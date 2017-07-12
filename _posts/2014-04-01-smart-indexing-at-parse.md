---
id: 2232
title: Smart Indexing at Parse
date: 2014-04-01T10:30:24+00:00
author: Charity Majors
layout: post
guid: http://blog.parse.com/?p=2232
permalink: /learn/engineering/smart-indexing-at-parse/
dsq_thread_id:
  - "3687483692"
categories:
  - Engineering
  - Ops
---
We love running databases at Parse! We also believe that mobile app developers shouldn't have to be DBAs to build beautiful, performant apps.

No matter what your data model looks like, one of the most important things to get right is your indexes. If your data is not indexed at all, every query will have to perform a full table scan to return even a single document. But if your data is indexed appropriately, the number of documents scanned to return a correct query result should be low.

We host over 200k mobile apps at Parse, so obviously we can not sit down and individually determine the correct indexes for each app. Even if we did, this would be an ineffective mechanism for generating indexes because developers can change their app schemas or query access patterns at any time. So instead we rely on our algorithmically generated smart indexing strategy.

We perform two types of index creation logic. The first generates simple indexes for each API request, and the second does offline processing to pick good compound indexes based on real API traffic patterns. In this blog post I will cover the strategies we use to determine simple single-column indexes. We will detail our compound indexing strategies in a followup post.

At a high level, our indexing strategy attempts to pick good indexes based on a) the order of operators' usefulness and b) the expected entropy of the value space for the key. We believe that every query should have an index; we choose to err on the side of too many indexes rather than too few. (Every index causes an additional write, but this is a problem that is easier to solve operationally than unindexed queries.)

Operator usefulness is determined primarily based on its effectiveness in providing the smallest search space for the query. The order of operators' usefulness is:

<ul class="standard-list">
  <li>
    equals
  </li>
  <li>
    <code>&lt;</code>, <code>&lt;=</code>, <code>=&gt;</code>, <code>&gt;</code>
  </li>
  <li>
    prefix string matches
  </li>
  <li>
    <code>$in</code>
  </li>
  <li>
    <code>$ne</code>
  </li>
  <li>
    <code>$nin</code>
  </li>
  <li>
    everything else
  </li>
</ul>

For data types, we heavily demote booleans, which have very low entropy and are not useful to index. We also throw out relations/join tables, since no values are stored for these keys. We heavily promote geopoints since MongoDB won't run a geo query without a geo index. Other data types are ranked by their expected entropy of the value space for the key. Data types in order of usefulness are:

<ul class="standard-list">
  <li>
    array
  </li>
  <li>
    pointers
  </li>
  <li>
    date
  </li>
  <li>
    string
  </li>
  <li>
    number
  </li>
  <li>
    other
  </li>
</ul>

We score each query according to the above metrics, and run `ensureIndex()` on the three top-scoring fields for each query. If the query includes a `$or`, we compute the top three indexes for each branch of the `$or`.

When we first started using smart indexing, all indexes were ensured inline with each API request. This created some unfortunate pile-on behavior any time a large app had schema or query pattern changes. Instead of ensuring the index in the request path, we now drop the indexing job in a queue where it is consumed by an indexer worker. This ensures that no more than one index can be generated at a time per replica set, which protects us from these pile-on effects.

Even the best indexing strategy can be defeated by suboptimal queries. Please see [Abhishek's primer](http://blog.parse.com/2014/03/27/efficient-parse-queries/) from last week on how to write queries that aren't terrible.