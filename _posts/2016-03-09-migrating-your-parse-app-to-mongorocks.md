---
id: 3959
title: Migrating Your Parse App to MongoRocks
date: 2016-03-09T11:10:04+00:00
author: travisredman
layout: post
guid: http://blog.parse.com/?p=3959
permalink: /learn/engineering/migrating-your-parse-app-to-mongorocks/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Engineering
  - Tutorial
tags:
  - mongodb
  - RocksDB
---
There are a lot of options out there for hosting your Parse MongoDB database. In our <a href="https://github.com/ParsePlatform/parse-server/wiki/Migrating-an-Existing-Parse-App" target="_blank">migration guide</a> we recommend that customers without database administration experience choose one of the hosted MongoDB providers such as <a href="http://docs.mlab.com/migrating-from-parse/" target="_blank">mLab</a> or <a href="https://objectrocket.com/parse" target="_blank">ObjectRocket</a>.

For those customers interested in running their own MongoDB infrastructure, we recommend running MongoDB with the RocksDB storage engine. MongoRocks has been in use by Parse for nearly a year, and offers a number of significant improvements over MMAP. Today we are releasing a guide to help you get started with MongoDB + RocksDB (MongoRocks for short). By migrating to MongoRocks, you will be running the same MongoDB backend that currently hosts your app.

## History

Parse was founded in 2011 and adopted MongoDB as the primary datastore for customer apps. At the time, MongoDB had a memory-mapped storage engine now known as MMAPv1. MMAP was suitable for initial development but became a source of pain as Parse began handling larger, more demanding workloads. Among the issues we encountered were:

<ul class="standard-list">
  <li>
     MMAP has a single writer lock per DB, severely limiting write throughput. (In 3.0, this is now a collection-level lock)
  </li>
  <li>
    No compression, requiring significantly more data storage
  </li>
  <li>
    Larger hardware costs due to memory requirements. With MMAP, memory is critical to performance, and you need a lot of it if you have a lot of data.
  </li>
  <li>
    No incremental backup solution. We had a solution based on EBS snapshots that was costly to implement.
  </li>
</ul>

To address these issues, we ultimately turned to <a href="http://www.rocksdb.org/" target="_blank">RocksDB</a>. RocksDB is an embedded, persistent key-value storage engine developed and open-sourced by Facebook. It is designed for high write throughput and storage efficiency. In 2015, MongoDB 3.0 was released, introducing the storage engine API, which enabled development of the <a href="https://github.com/mongodb-partners/mongo-rocks" target="_blank">MongoRocks</a> storage engine for MongoDB. MongoRocks combines the extensive features of MongoDB with the speed and efficiency of RocksDB, creating a very powerful noSQL database solution.

## Greater Performance and Efficiency

Parse migrated all of its app database storage to MongoRocks in mid-2015, after seeing <a href="http://blog.parse.com/learn/engineering/mongodb-rocksdb-writing-so-fast-it-makes-your-head-spin/" target="_blank">dramatically improved performance</a> in benchmarks over the original MMAP storage engine. Some highlights include:

<ul class="standard-list">
  <li>
     50x increased write speed over MMAP
  </li>
  <li>
    An average of 90% reduction in storage space over MMAP
  </li>
  <li>
    Document-level locking for increased throughput
  </li>
</ul>

With MongoRocks, Parse was able to more effectively utilize its existing infrastructure, in some cases running over twice the number of apps on the same replica set while also improving average performance.

## Easier Backups

RocksDB's LSM-based data structure enables backups to be easy and efficient. Using the <a href="http://blog.parse.com/learn/engineering/strata-open-source-library-for-efficient-mongodb-backups/" target="_blank">strata</a> tool that we open-sourced last year, and Amazon S3, you can have backups going in a matter of minutes. Backups with strata:

<ul class="standard-list">
  <li>
     Are incremental, requiring less time and storage.
  </li>
  <li>
    Require no downtime, and can be run frequently
  </li>
  <li>
    Scale easily as the size of your DB grows
  </li>
</ul>

At Parse, strata-based backups were a huge win. Compared to our EBS-based approach for MMAP, we saw an 85% reduction in base backup and restore times, and a 60% reduction in incremental backup times. Since the backups could be run without downtime or service degradation, we eliminated our dedicated backup hosts, cutting our infrastructure footprint by 25% and total DB costs (including storage) by a similar amount.

You can consult the <a href="https://github.com/ParsePlatform/parse-server/wiki/MongoRocks#backups" target="_blank">backups</a> section of our guide for information on how to configure strata.

## Guide

You can find the MongoRocks setup guide <a href="https://github.com/ParsePlatform/parse-server/wiki/MongoRocks" target="_blank">here</a>. If you have questions or feedback, please visit the <a href="https://groups.google.com/forum/#!forum/mongo-rocks" target="_blank">MongoRocks Google Group</a>.