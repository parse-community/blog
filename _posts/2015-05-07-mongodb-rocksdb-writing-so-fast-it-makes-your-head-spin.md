---
id: 3504
title: 'MongoDB + RocksDB: Writing so Fast it Makes Your Head Spin'
date: 2015-05-07T09:35:00+00:00
author: Mike Kania
layout: post
guid: http://blog.parse.com/?p=3504
permalink: /learn/engineering/mongodb-rocksdb-writing-so-fast-it-makes-your-head-spin/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3744774481"
categories:
  - Engineering
tags:
  - mongodb
  - RocksDB
---
[Last week](http://blog.parse.com/learn/engineering/mongodb-rocksdb-benchmark-setup-compression/), I talked about how we set up Flashback to start benchmarking MongoDB 3.0 and highlighted the insane storage efficiencies achieved with both RocksDB and WiredTiger. As a recap, the amount of storage used dropped by more than **10x** when importing a production replica set into either RocksDB or WiredTiger.

This next post will dive into how MongoDB with RocksDB performs against production workloads.

We initially focused on latencies, which were broken down by operation type. For comparison, we ran benchmarks against three replica sets. The first ran MongoDB 2.6 as a control because it's the same version we run in production. Next, we had a MongoDB 3.0 replica set running on MMAPv1. This was to call attention to any performance regressions/improvements between 2.6 and 3.0 that are independent of the underlying storage engine. Lastly was the new MongoDB 3.0 replica set running on RocksDB.

&nbsp;

<img class="aligncenter size-full wp-image-3505" src="{{ site.url }}/assets/wp-content/uploads/2015/05/flashback_diagram.png" alt="flashback_diagram" width="463" height="207" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/flashback_diagram.png 463w, {{ site.url }}/assets/wp-content/uploads/2015/05/flashback_diagram-300x134.png 300w" sizes="(max-width: 463px) 100vw, 463px" />

## No Results for WiredTiger

Sadly, we weren’t able to get a successful benchmark of WiredTiger with this replica set, which contained around 370k collections. WiredTiger maps each collection to a file on disk, so our idle node automatically had 370k open file descriptors. Along with having way too many file descriptors, startup time took well over an hour. When we attempted to run a Flashback replay, the WiredTiger replica set ran into a series of write stalls. Eventually, mongod crashed due to the hazard pointer table being full. This turned out to be a bug that has since been [fixed](https://jira.mongodb.org/browse/SERVER-17551). WiredTiger would need to make some changes to better support large collection counts, but for this benchmark we will be testing without it.

## Running the Benchmark

Parse runs all kinds of workloads on MongoDB, from read-heavy to write-heavy and everything in between, depending on the replica set. This specific workload turned out to be more read- than write-heavy. This will have interesting implications for RocksDB because it's optimized for more write-heavy workloads.
  
&nbsp;
  
<img class="aligncenter size-full wp-image-3506" src="{{ site.url }}/assets/wp-content/uploads/2015/05/flashback_counts_by_op_type.png" alt="flashback_counts_by_op_type" width="671" height="502" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/flashback_counts_by_op_type.png 671w, {{ site.url }}/assets/wp-content/uploads/2015/05/flashback_counts_by_op_type-300x224.png 300w" sizes="(max-width: 671px) 100vw, 671px" />
  
We ran the replay portion of Flashback using the `-style=real` flag. This flag will use the timestamp of each captured operation and replay it at the same rate as executed during the capture.

<pre class="line-numbers"><code class="language-bash">flashback \
-ops_filename=OUTPUT \
-style=real \&lt;code>
-url=$MONGO_HOST:27017 \
-workers=50</code></pre>

</code>

### Results

Most of the operations ran in 1 millisecond(ms) or fewer according to the MongoDB logs. Unfortunately, the logs report operation latency as an integer. So any operation executed in under a millisecond is outputted as 0ms in the MongoDB logs. This will skew some of our average latency calculations towards the slower operations.

We also [discovered](https://groups.google.com/forum/#!searchin/mongodb-dev/lock/mongodb-dev/l2fR4Xr-Bxo/vgwWh8t55JAJ) that in MongoDB 2.6, the time waiting on a lock for the **first time** is not included in the overall latency. MongoDB 3.0 changed this behavior to include initial lock time. This would have incorrectly shown MongoDB 2.6 to perform better than it actually was. We ended up [patching](https://github.com/mongodb-partners/mongo/commit/93a406c23ea6c0dd553f5dbd8ff016451a7f3673) our test version of MongoDB 2.6 to include the initial lock time in the overall latency.

### Key Findings

<ul class="standard-list">
  <li>
    All write operations in RocksDB were under 10ms.
  </li>
  <li>
    On average, RocksDB outperforms 2.6 and 3.0 MMAPv1 for every operation type except queries.
  </li>
  <li>
    In the P99 latencies, RocksDB either is on par or outperforms 2.6 and 3.0 MMAPv1 for every operation type except counts.
  </li>
  <li>
    Regardless of storage engine, nearSphere queries have performance regressions on MongoDB 3.0.
  </li>
  <li>
    Capped collections are inefficient on RocksDB.
  </li>
</ul>

<img class="aligncenter size-full wp-image-3507" src="{{ site.url }}/assets/wp-content/uploads/2015/05/flashback_latency.png" alt="flashback_latency" width="814" height="435" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/flashback_latency.png 814w, {{ site.url }}/assets/wp-content/uploads/2015/05/flashback_latency-300x160.png 300w" sizes="(max-width: 814px) 100vw, 814px" />

## Writes are blindingly fast

In a [<u>Log-Structure-Merge Tree </u>](http://en.wikipedia.org/wiki/Log-structured_merge-tree)implementation like RocksDB, writes are done in memory, which makes these operations incredibly fast no matter what the dataset looks like. On RocksDB the findandmodify, update, and insert operations all dropped to single-digit milliseconds for both average and P99 latencies. In total, all write operations executed in less than 10ms. Having RocksDB really saved our ass for write-heavy customers, which will be the topic of our next blog post.

## RocksDB reads are Complicated

We did find that queries on average were slower with RocksDB, but P99 latencies were faster. The RocksDB team spent some time understanding slower queries. They found that the biggest difference in query latencies between MMAPv1 and RocksDB happen in the following situations:

<ol class="standard-list">
  <li>
    If the entire working set of a query is cached
  </li>
  <li>
    When the number of documents scanned for a query is large
  </li>
</ol>

When a query is cached in memory, MMAPv1 is faster than RocksDB. This is because reading a document from memory can be done with a single lookup on MMAPv1. RocksDB has to search more places in memory to return the same document. On the flip side, if the working set does not fit in memory on MMAPv1, RocksDB is faster. This is because RocksDB is able to fit more of the working set in memory thanks to the storage savings that RocksDB has over MMAPv1.

In the case where only a single document is scanned, 2.6 MMAPv1 is in most cases waiting for a database-level lock. Waiting for the lock can be slower than reading a single document in RocksDB because RocksDB supports document-level locking. Collection-level locking in 3.0 MMAPv1 improves this behavior and we can see that from the P99 query latencies. Scanning more documents eventually offsets the upfront cost of waiting for a database lock in MMAPv1.

Below we have a set of scatterplots showing the number of documents scanned with overall operation latency in milliseconds. As the number of documents scanned increases in 2.6 MMAPv1, we can see latencies cluster between 0-10ms regardless of the number of documents scanned. With RocksDB, query latency grows linearly with the number of documents scanned.
  
&nbsp;
  
<img class="aligncenter size-full wp-image-3508" src="{{ site.url }}/assets/wp-content/uploads/2015/05/mongo26_scatterplot.png" alt="mongo26_scatterplot" width="939" height="436" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/mongo26_scatterplot.png 939w, {{ site.url }}/assets/wp-content/uploads/2015/05/mongo26_scatterplot-300x139.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/mongo26_scatterplot-875x406.png 875w" sizes="(max-width: 939px) 100vw, 939px" />

<img class="aligncenter size-full wp-image-3509" src="{{ site.url }}/assets/wp-content/uploads/2015/05/mongo30_rocks_scatterplot.png" alt="mongo30_rocks_scatterplot" width="939" height="437" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/mongo30_rocks_scatterplot.png 939w, {{ site.url }}/assets/wp-content/uploads/2015/05/mongo30_rocks_scatterplot-300x140.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/mongo30_rocks_scatterplot-875x407.png 875w" sizes="(max-width: 939px) 100vw, 939px" />

## Performance Regressions between 2.6 and 3.0

In the course of running this benchmark, we did notice a performance regression when issuing nearSphere queries using 2d indexes. The initial problem we had was with collections with dense location data points, which is common in major cities. The team at MongoDB figured out that in 3.0 the initial search area for nearSphere queries was too wide for this case. MongoDB was able to [patch](https://jira.mongodb.org/browse/SERVER-17469) and release the fix in 3.0.2.

While this worked in the case of simple nearSphere queries, we still see performance regressions with more complex nearSphere queries. MongoDB acknowledges the regressions, and have [suggested](https://jira.mongodb.org/browse/SERVER-18056) using `$near` as a workaround. We are still investigating if this is a reliable workaround.

## Capped Collections perform better on MMAPv1

Maintaining a fixed number of documents in a capped collection is very cheap on MMAPv1. A capped collection is treated like a circular buffer where the space is pre-allocated as soon as the collection is created. While this consumes unused disk space, inserts are very efficient.

Capped collections in RocksDB require some overhead. First, RocksDB doesn't pre-allocate space for capped collections. When an insert operation is issued, RocksDB first needs to perform a read to determine the number of documents in the capped collections. If the collection has reached the maximum number of documents, a delete also needs to be made. As more documents are deleted, the longer reads will take in a follow up insert.

Cassandra, a similar LSM database, shares this same problem and the folks at DataStax made a [detailed post](http://www.datastax.com/dev/blog/cassandra-anti-patterns-queues-and-queue-like-datasets) about it.

## From Here to Running in Production

The final blog post in this series will talk about the steps we took to get ready to support RocksDB in production and the insane performance improvements that resulted. Stay tuned.