---
id: 3478
title: 'MongoDB + RocksDB: Benchmark Setup &amp; Compression'
date: 2015-04-30T10:00:31+00:00
author: mikekania
layout: post
guid: http://blog.parse.com/?p=3478
permalink: /learn/engineering/mongodb-rocksdb-benchmark-setup-compression/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - large
dsq_thread_id:
  - "3725884852"
categories:
  - Engineering
  - Ops
tags:
  - mongodb
  - RocksDB
---
Parse has been running on MongoDB since version 1.8, so we’ve accumulated a massive amount of operational knowledge about what it takes to run a production MongoDB deployment. Running MongoDB is fairly easy operationally; however, scaling MongoDB can be challenging with things like storage inefficiencies and database-level lock contention. The new modular storage engine API in MongoDB 3.0 is like a magical wand that enables us to run MongoDB on a storage engine optimized for our workloads.

As we said [last week](http://blog.parse.com/announcements/mongodb-rocksdb-parse/), this will be the first in a series of posts where we do a deep dive into our experiences benchmarking MongoDB 3.0 with RocksDB. Parse has had the tremendous benefit of working side by side with the [RocksDB](http://rocksdb.org/) team, who have engineered a screaming fast write-optimized storage engine that is already being used to power multiple internal Facebook services.

## Benchmark Setup

[Flashback](https://github.com/ParsePlatform/flashback) is a tool we open-sourced last year to record our production workloads and replay them in an isolated environment at varying speeds. We evaluated performance using Flashback’s per-operation latency metrics. In addition, we also set up a log processing pipeline that allowed us to perform ad-hoc analysis and diagnose performance regressions. At a high level, the pipeline works by first logging every operation in MongoDB, and then parsing the log lines into JSON using [<span style="text-decoration: underline;">mongologtools</span>](https://github.com/tmc/mongologtools). The JSON is then shipped to an internal Facebook data diving tool that we used to analyze performance in realtime.

To come up with a replayable production workload, we need to do two things: start capturing all operations of a replica set, and create a consistent snapshot from the start of the recorded session.

### Capturing a workload

First we need to begin capturing all the production operations for a set period of time by initiating a “record” session using Flashback. To set up a Flashback record session, we copied the config template (config.template) and modified the following stanzas:

<ul class="standard-list">
  <li>
    <b>target_databases</b>: A list of all the databases you would like to record
  </li>
  <li>
    <b>oplog_server</b>: A secondary that will be used to tail the oplog for write operations
  </li>
  <li>
    <b>profiler_server</b>: The primary in the target replica set to capture profiling data
  </li>
  <li>
    <b>duration_sec:<b> </b></b>Defines how long you want to record
  </li>
</ul>

We recommend setting duration_sec to a long time, like 8 to 12 hours, so you get a representative sample of your requests.

Next, we enabled profiling on the primary target node using the set\_mongo\_profiling.py script. Warning: this **can** impact the performance of your production replica set because it does an additional write for each operation to the profiling collection, although in practice this has generally been fine for us.

<pre class="line-numbers"><code class="language-bash">./set_mongo_profiling.py -a enable -n $PRIMARY_HOSTNAME</code></pre>

Now we kick off a record session using

<pre class="line-numbers"><code class="language-bash">./record.py</code></pre>

Once the record process has finished, there will be a large JSON file with all of the recorded operations in the order they were executed.

### Creating a Consistent Snapshot

Around the same time you start recording operations, you need to generate a consistent backup that you can restore in your isolated test environment. Parse uses EBS snapshots, so generating an initial backup for us is as simple as locking mongod, creating an EBS snapshot of all the RAIDed volumes on /var/lib/mongodb, and unlocking mongod.

### Quickly Replaying the same Workload

After replaying a workload, we wanted an efficient way to roll back to a consistent state. We could restore back to the snapshotted EBS volumes each time, but this is very time-consuming because you also need to warm up the EBS volumes and pull the blocks down from S3 for each fresh run. This can take hours or days if you have terabytes of data.

With that in mind, we decided to use LVM on top of EBS to provide a copy-on-write restore point that can be easily rolled back. Although this will incur a small amount of I/O overhead, we can now replay the same workload multiple times quickly and easily. As part of our production provisioning process, we create the logical volumes on top of the RAIDed set of EBS volumes.

<pre class="line-numbers"><code class="language-bash">#/dev/md0 is the EBS RAID
pvcreate /dev/md0
vgcreate mongovg /dev/md0
lvcreate -l 90%%VG -n mongoraid mongovg</code></pre>

When restoring from backup in our test environment, we define a restore point:

<pre class="line-numbers"><code class="language-bash">lvcreate -l 10%VG -s -n restore_point /dev/mongovg/mongoraid</code></pre>

After finishing a benchmark, we stop mongod, unmount the filesystem and merge the copy-on-write logical volume to rollback to a consistent state:

<pre class="line-numbers"><code class="language-bash">lvconvert --merge /dev/mongovg/restore_point</code></pre>

It's a simple wash, rinse, and repeat to replay the same workload with different tuning parameters.

## Benchmarking Multiple Storage Engines

Normally when we run Flashback benchmarks, we can just restore from backup and replay our captured workload over and over. However in MongoDB 3.0, each storage engine has a different on-disk format. So we also need to run an initial sync of each new storage engine against our restored MMAPv1 backup, and then run benchmarks on each format.

### Storage engine efficiency results

We ran an initial sync on 3.0MMAPv1, RocksDB and WiredTiger from our production backup. This particular replica set contained approximately 150 databases and 370k collections. Both WiredTiger and RocksDB had compression enabled using Snappy. The test hosts ran on EC2. We used r3.4xlarge instances with 6TB of EBS storage and 6000 provisioned IOPS.

<img class="aligncenter wp-image-3498 size-content-full" src="{{ site.url }}/assets/wp-content/uploads/2015/04/mongo3.0-storage-efficiency-875x496.png" alt="mongo3.0-storage-efficiency" width="640" height="363" srcset="{{ site.url }}/assets/wp-content/uploads/2015/04/mongo3.0-storage-efficiency-875x496.png 875w, {{ site.url }}/assets/wp-content/uploads/2015/04/mongo3.0-storage-efficiency-300x170.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/04/mongo3.0-storage-efficiency.png 939w" sizes="(max-width: 640px) 100vw, 640px" />

After the initial sync, RocksDB and WiredTiger cut storage used by more than 10x when compared to MMAPv1, which is pretty insane. Roughly half of the entire data set can now fit in memory (144GB). So for both WiredTiger and RocksDB, an operation that would normally require a disk read in MMAPv1 is more likely to be a simple memory lookup.

## Next Steps

After getting over the shock and delight of potentially saving **10x** on our storage costs, we wanted to focus on operational latencies and throughput. The second post in this series will be out next week with results of the benchmarking runs broken down by query type.