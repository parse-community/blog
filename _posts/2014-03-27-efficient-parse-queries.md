---
id: 2194
title: Writing Efficient Parse Queries
date: 2014-03-27T10:56:41+00:00
author: abhishekkona
layout: post
guid: http://blog.parseplatform.org/?p=2194
permalink: /learn/engineering/efficient-parse-queries/
dsq_thread_id:
  - "3687773395"
categories:
  - Engineering
  - Ops
---
Any time you store objects on Parse, they get stored on a database. Usually, a database stores a bag of data on disk, and when you issue Parse queries it looks up the data and returns the matching objects. Since it doesn't make sense for the database to look at all the data present in a particular Parse class for every query, the database uses something called an index. An index is a sorted list of items matching a given criteria. Indexes help because they allow the database to do an efficient search and return matching results without looking at all of the data. For the more algorithmically inclined, this is an _O(log(n)) vs O(n)_ tradeoff. Indexes are typically smaller in size and fit in memory, resulting in faster lookups.

There are a couple of operations - `$ne` (Not Equal To) and `$nin` (Not In) which cannot be supported by indexes. When you run a query with a “Not Equal To” clause, the database has to look at all objects in the particular Parse class. Consider an example: we have a column "Color" in our Parse Class. The index helps us know which objects have "Color" blue, green and so on. When running the query:

<pre class="brush: python; gutter: false">{ "Color" : { "$ne" : "red" } }</pre>

The index is useless, as it can only tell which objects have a particular color. Instead, the database has to look at all the objects. In database language this is called a "Full Table Scan" and it causes your queries to get slower and slower as your database grows in size.

Luckily, most of the time a `$ne` query can be rewritten as a `$in` query. Instead of querying for the absence of values, you ask for values which match the rest of the column values. Doing this allows the database to use an index and your queries will be faster.

For example if the _"User"_ object has a column called _"State"_ which has values _"SignedUp", "Verified", and "Invited",_ the slow way to find all users who have used the app at least once would be to run the query:

<pre class="brush: python; gutter: false">{ "State" : { "$ne" : "Invited" } }</pre>

It would be faster to use the `$in` version of the query:

<pre class="brush: python; gutter: false">{ "State" : { "$in" : [ "SignedUp", "Verified" ] }</pre>

In a similar vein a `$nin` query is bad, too, as it also can't use an index. You should always try to use the complimentary $in query. Building on the above example, if the "_State_" column had one more value, "_Blocked",_ to represent blocked users, a slow query to find active users would be:

<pre class="brush: python; gutter: false">{ "State" : { "$nin" : [ "Blocked", "Shadow" ] } }</pre>

Using a complimentary `$in` query will be always be faster:

<pre class="brush: python; gutter: false">{ "State" : { "$in" : [ "Active", "Verified" ] } }</pre>

We're working hard here at Parse so that you don't have to worry about indexes and databases, and knowing some of the tradeoffs in using the `$ne`, `$nin` operators goes a long way in making your app faster.