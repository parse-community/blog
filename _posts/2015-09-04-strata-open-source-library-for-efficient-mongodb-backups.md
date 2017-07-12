---
id: 3779
title: 'Strata: Open Source Library for Efficient MongoDB Backups'
date: 2015-09-04T08:00:15+00:00
author: travisredman
layout: post
guid: http://blog.parse.com/?p=3779
permalink: /learn/engineering/strata-open-source-library-for-efficient-mongodb-backups/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4095638922"
categories:
  - Engineering
  - Ops
tags:
  - go
  - mongodb
  - ops
  - RocksDB
---
If you saw Charity's [post](http://blog.parse.com/announcements/mongodb-rocksdb-parse/) back in April, you'll recall that Parse is using MongoDB built on RocksDB ("MongoRocks") in our production MongoDB deployment. MongoRocks combines the MongoDB storage API and RocksDB storage engine to deliver faster writes and more efficient storage utilization compared with MMAP. At Parse we've seen write speed improvements of up to 50x and up to a 90% reduction in disk space. As we have hardened operations and tooling around MongoRocks deployments, we've found another great advantage: it can make doing full and incremental backups incredibly easy!

* * *

## Backups on Mongo with MMAPv1

Taking backups of our MongoDB replica sets has always been a source of pain at Parse. With a large amount of data (several TB per replica set), it's not reasonable for us to use the mongodump command. It takes hours or days to generate a complete dump file, which must then be shipped off to remote storage. A lack of incremental support also means that we need to store duplicate data, and generate a full dump each time.

Until now, our solution has been to use Amazon EBS snapshots. This has made the process relatively simple:

<ol class="standard-list">
  <li>
    Issue a db.fsyncLock() and fsync on the backup host
  </li>
  <li>
    Trigger an EBS snapshot on all EBS volumes in the RAID
  </li>
  <li>
    Store some metadata consisting of timestamp and the snapshot IDs involved in this backup
  </li>
  <li>
    Issue a db.fsyncUnlock()
  </li>
</ol>

Triggering a snapshot this way takes a matter of seconds. Afterward, the heavy lifting occurs on Amazon's side, which means there's little to orchestrate on our end. After a base snapshot has been created (many hours), an incremental snapshot will usually complete in a matter of minutes. To restore, we simply pull the snapshots for the given timestamp we want.

This has worked for us for a while, but has a few shortcomings:

### Dedicated Infrastructure

EBS snapshots put a lot of stress on the EBS volume while in progress, and the backup node frequently lags behind the replica set until the snapshot is complete. This means the node is unusable for query traffic during the backup window, and we have had to dedicate an entire node in each replica set just for backups. This is a lot of hardware to have just sitting around.

### Slow Restore Times

Provisioning volumes from an EBS snapshot is fast, but to have production-ready volumes, they need to be warmed, and this can take up to 24H (with 1TB volumes). This delay is expensive and slows us down when we want to rapidly access restored data, such as in [benchmarking](http://blog.parse.com/learn/engineering/mongodb-rocksdb-benchmark-setup-compression/) or in a potential DR scenario.

### Cost

To use EBS snapshots we have to pay for both the storage of tens of thousands of snapshots and the underlying EBS PIOPS volumes.

## Backups with LSM

RocksDB's LSM (Log-structured Merge-tree) data format introduces possibilities for doing backups very efficiently. To understand this, let's look at some key characteristics of LSM:

<ul class="standard-list">
  <li>
    LSM writes data first to its memtable. When this needs to be flushed to disk, it is written as an .sst file.
  </li>
  <li>
    After an .sst file is written, it is immutable. It will never be modified again, but may be removed during a compaction operation
  </li>
  <li>
    Compaction is run periodically to process updated and deleted keys, with older .sst files being removed and newer .sst files being created
  </li>
  <li>
    A description of all .sst files is maintained in a MANIFEST file stored with the .sst files
  </li>
</ul>

Our approach to backups takes advantage of the immutable nature of .sst files. Here's a simplified example.

A working MongoRocks backup may look something like this:

<pre class="line-numbers"><code class="language-bash">- MANIFEST-000001
- CURRENT
- 0000.sst
- 0001.sst</code></pre>

To restore this DB to its current state, we need a copy of all files. Let's assume we've copied these files for a moment, and call it backup #1. Some time later, more writes occur and the directory now looks like this:

<pre class="line-numbers"><code class="language-bash">- MANIFEST-000002
- CURRENT
- 0000.sst
- 0001.sst
- 0002.sst</code></pre>

To make a backup (backup #2) of this state, we again need all of the files. However, since **0000.sst** and **0001.sst** were captured in the previous backup, we can skip them, since we know they haven't changed.

Some more time has passed, a compaction has run, and the directory now looks like this:

<pre class="line-numbers"><code class="language-bash">- MANIFEST-000003
- CURRENT
- 0002.sst
- 0003.sst</code></pre>

Since we copied **0002.sst** in backup #2, we don't need to copy it in backup #3. What about 0000.sst and 0001.sst? We still have copies of them in our remote storage. If we care about restoring from backup #1 or #2, we need to keep these copies around. Otherwise, they can be safely deleted. LSM allows backup operations to be very efficient since only new, compacted data is copied.

In practice, production databases can have tens of thousands of .sst files, but the approach is still the same. After you have a base copy of your data, maintaining an up-to-date backup is simply a matter of recognizing and copying new files while pruning obsolete files.

## Backup of MongoRocks

Copying LSM files is straightforward, but how do you get a consistent copy of all of the LSM files on a live database? MongoRocks has a helpful command for getting a complete point-in-time snapshot of the database state.

<pre class="line-numbers"><code class="language-bash">db.adminCommand({setParameter:1, rocksdbBackup: "/path/to/backup/dir"})</code></pre>

<span style="line-height: 1.5">When running this command, RocksDB flushes its memtables to .sst files, creates hard links to the current .sst files, and copies the MANIFEST and CURRENT files to the backup directory. This operation takes a matter of seconds, and afterward a backup agent can simply copy the contents of the backup directory to get a complete picture of the DB at the time the snapshot was taken. When done copying, just delete the backup directory!</span>

What do we gain from this approach?

### Faster Backup and Restore Times

Both backup and restore times are reduced when compared to EBS snapshots. While EBS requires a full block-level copy of the storage volume to build a base snapshot, backing up LSM files only requires transferring the existing file data. In practice, it takes significantly less time to copy this data than to copy all blocks on the volume.

Similarly, restoring from backup means simply pulling files from remote storage, which is usually a lot less data and requires less time to do than pulling all blocks down from an EBS snapshot. On average it takes 2-3 hours to restore a complete replica set from S3, versus nearly 24 hours to pull all blocks down from an EBS snapshot. We also see more consistent incremental backup performance, with incremental MongoRocks backups taking an average of 5 minutes versus a range of 15-30 minutes with EBS.

 <img class="alignnone size-full wp-image-3788" src="{{ site.url }}/assets/wp-content/uploads/2015/09/base_backup.png" alt="base_backup" width="346" height="279" srcset="{{ site.url }}/assets/wp-content/uploads/2015/09/base_backup.png 346w, {{ site.url }}/assets/wp-content/uploads/2015/09/base_backup-300x242.png 300w" sizes="(max-width: 346px) 100vw, 346px" /> <img class="alignnone size-full wp-image-3789" src="{{ site.url }}/assets/wp-content/uploads/2015/09/inc_backup.png" alt="inc_backup" width="346" height="269" srcset="{{ site.url }}/assets/wp-content/uploads/2015/09/inc_backup.png 346w, {{ site.url }}/assets/wp-content/uploads/2015/09/inc_backup-300x233.png 300w" sizes="(max-width: 346px) 100vw, 346px" /><img class="alignnone size-full wp-image-3790" src="{{ site.url }}/assets/wp-content/uploads/2015/09/restore_time.png" alt="restore_time" width="348" height="269" srcset="{{ site.url }}/assets/wp-content/uploads/2015/09/restore_time.png 348w, {{ site.url }}/assets/wp-content/uploads/2015/09/restore_time-300x232.png 300w" sizes="(max-width: 348px) 100vw, 348px" />

&nbsp;

### Reduced Storage Costs

Since only new .sst files are saved, we see significant storage savings by only paying for storage for data that has changed. Additionally, since we no longer need EBS snapshots, we can use faster local SSDs, and do not need to pay the additional cost for EBS PIOPS volumes.

### Less Infrastructure

We no longer need a dedicated, hidden backup host, which means we can remove a replica from each replica set and reduce infrastructure cost and complexity.

* * *

## Introducing Strata

We're so excited about this approach to backups that we are releasing **<a href="http://github.com/facebookgo/rocks-strata" target="_blank">rocks-strata</a>**, our Go-based library and command line toolset for performing, managing, and restoring MongoRocks-based backups. Some features include:

<ul class="standard-list">
  <li>
    Simple CLI tool to trigger backups on the local host, copy files and file metadata to remote storage, query metadata, and perform local restores
  </li>
  <li>
    Amazon S3 remote storage implementation (can be replaced by an alternative storage system of your choice by implementing the Storage interface)
  </li>
  <li>
    Query-able remote backups: you can query remote backups from a mongo shell without performing a full restore.
  </li>
</ul>

Special thanks to Facebook intern [Aaron Feldman](https://github.com/AGFeldman) for building out this toolset over the past couple of months. If you are using or experimenting with MongoRocks, we encourage you to give it a try!