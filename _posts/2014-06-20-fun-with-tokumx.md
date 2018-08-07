---
id: 2358
title: Fun with TokuMX
date: 2014-06-20T22:37:42+00:00
author: kailiu
layout: post
guid: http://blog.parseplatform.org/?p=2358
permalink: /learn/engineering/fun-with-tokumx/
dsq_thread_id:
  - "3678708617"
categories:
  - Engineering
  - Ops
---
**[TokuMX](http://www.tokutek.com/)** is an open source distribution of MongoDB that replaces the default B-tree data structure with a [fractal tree index](http://www.tokutek.com/resources/technology/), which can lead to dramatic improvements in data storage size and write speeds. [Mark Callaghan](http://www.blogger.com/profile/09590445221922043181) made a series of awesome [blog posts](http://smalldatum.blogspot.com/) on benchmarking InnoDB, TokuMX and MongoDB, which demonstrate TokuMX's remarkable write performance and extraordinarily efficient space utilization. We decided to benchmark TokuMX against several real-world scenarios that we encountered in the Parse environment. We also built a set of tools for capturing and replaying query streams. We are open sourcing these tools on [github](https://github.com/ParsePlatform/flashback) so that others may also benefit from them (we'll discuss more about them in the last section).

In our benchmarks, we tested three aspects of TokuMX: 1. Exporting and importing large collections; 2. Performance for individual write-heavy apps; and 3. Database storage size for large apps.

## **1. Importing Large Collections**

* * *

## 

We frequently need to migrate data by exporting and importing collections between replica sets. However, this process can be painful because sometimes the migration rate is ridiculously slow, especially for collections with a lot of small entries and/or complicated indexes. To test importing and exporting, we performed an import/export on two representative large collections with varying object counts.

<ul class="standard-list">
  <li>
    <i> Collection1: </i>143 GB collection with ~300 millions of small objects
  </li>
  <li>
    <i> Collection2:</i> 147 GB collection with ~500 thousands of large objects
  </li>
</ul>

Both collections are exported from our existing MongoDB collections, where collection1 took **6 days** to** ******export and collection2 took **6 hours**. We used the mongoimport ****command to import collections to MongoDB and TokuMX instances. Benchmark results for importing collection1, with a large number of small objects: TokuMX is 3x faster to import.

<pre class="brush: c; gutter: false"># Collection1: exported from MongoDB for 6 days

Database         Import Time
---------------------------------------------------------------------
MongoDB           58 hours 37 minutes
TokuMX            14 hours 28 minutes</pre>

Benchmark results for importing collection2, with a small number of large objects: TokuMX and MongoDB are roughly in parity.

<pre class="brush: c; gutter: false"># Collection2: exported from MongoDB for 6 hours

Database         Import Time
---------------------------------------------------------------------
MongoDB           48 minutes
TokuMX            53 minutes</pre>

## **2. Handling Heavy Write Loads**

* * *

## 

One of our sample write-intensive apps issues a heavy volume of “update” requests with large object sizes. Since TokuMX is a write-optimized database, we decided to benchmark this query stream against both MongoDB and TokuMX. We recorded 10 hours of sample traffic, and replayed it against both replica sets. From the benchmark results, TokuMX performs 3x faster for this app with much smaller latencies at all histogram percentiles.

<pre class="brush: c; gutter: false"># MongoDB Benchmark Results
- Ops/sec: 1695.81
- Update Latencies:
    P50: 5.96ms
    P70: 6.59ms
    P90: 11.57ms
    P95: 18.40ms
    P99: 44.37ms
    Max: 102.52ms</pre>

<pre class="brush: c; gutter: false"># TokuMX Benchmark Results
- Ops/sec: 4590.97
- Update Latencies:
    P50: 3.98ms
    P70: 4.49ms
    P90: 6.33ms
    P95: 7.61ms
    P99: 12.04ms
    Max: 16.63ms</pre>

## **3. Efficiently Using Disk Space**

* * *

## 

Space efficiency is another big selling point for TokuMX. How much can TokuMX save in terms of disk utilization? To figure this out, we exported the data of one of our shared replica sets (with 2.4T ****data in total) and imported them into TokuMX instances. The result was stunning: TokuMX used only 379G disk space —**about 15% of the original size.**

## **Benchmark Tools**

Throughout the benchmarks, we focused on:

<ul class="standard-list">
  <li>
    Using “real” query patterns to evaluate the database performance
  </li>
  <li>
    Figuring out the maximal performance of the systems
  </li>
</ul>

To achieve those goals, we developed a tool, [flashback,](https://github.com/ParsePlatform/flashback) that records the real traffic to the database and replays ops with different strategies. You can replay ops either as fast as the database can accept them, or according to their original timestamp intervals. We are open sourcing this tool because we believe it will be also useful for people who are interested in recording their real traffic and replaying it against different production environments, such as for smoke testing version upgrades or different hardware configurations. For more information on using flashback, please refer to this [document](https://github.com/liukai/flashback/blob/master/README.md). We accept pull requests!