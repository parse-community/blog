---
id: 2090
title: Parse Analytics is Getting Smarter
date: 2014-02-20T21:50:17+00:00
author: listiarsowastuargo
layout: post
guid: http://blog.parseplatform.org/?p=2090
permalink: /announcements/parse-analytics-is-getting-smarter/
dsq_thread_id:
  - "3671209783"
categories:
  - Announcements
---
Here at Parse we're constantly working to improve the experience of developers creating and managing applications. Today, we're happy to announce that Parse Analytics is getting smarter by adding two new components, Data Analytics and Performance Analytics.

**Data**

On Parse you can manage your collection in our Data Browser — but sometimes you might wonder how your collection grows over time. In the past, you could see growth by manually filtering your API Requests for `CREATE` requests on a particular class. We've made it a lot easier to see the growth of each collection over time:

<p style="text-align: center;">
  <a href="{{ site.url }}/assets/wp-content/uploads/2014/02/Data-Analytics.png"><img class=" wp-image-2110 aligncenter" alt="Data Analytics" src="{{ site.url }}/assets/wp-content/uploads/2014/02/Data-Analytics-1024x518.png" width="759" height="384" /></a>
</p>

With our new Data section, you have free access to metrics like user signups. If you have a class that stores photo or message metadata, this is an easy way to track their growth.

**Performance**

Under the hood at Parse, every app has a limit on concurrent requests per second. Apps on our current free Basic plan are allowed 20 requests/sec and Pro plans come with 40 requests/sec. Though the traffic of most apps is well under this limit, we now have much finer-granularity data so you can see how close your app comes to hitting this limit: Performance Analytics have come to the rescue!

Below, you can see the Performance graph of an app which hits its burst limit frequently (a Pro plan's 40 requests/sec is shown on this graph as 2400 requests/min). As requests exceed the app's burst limit, Parse will begin dropping requests and returning an error with error code 155. Yours might look very different - you can remove the Burst Limit drop-down in order to see Total Requests, Dropped Requests, or Served Requests on their own.

<p style="text-align: center;">
  <a href="{{ site.url }}/assets/wp-content/uploads/2014/02/Performance-Analytics.png"><img class=" wp-image-2108 aligncenter" alt="Performance Analytics" src="{{ site.url }}/assets/wp-content/uploads/2014/02/Performance-Analytics-1024x483.png" width="759" height="358" /></a>
</p>

Parse Analytics is a work in progress and we're busy adding more exciting tools to the offering. Try it today and let us know what you think!