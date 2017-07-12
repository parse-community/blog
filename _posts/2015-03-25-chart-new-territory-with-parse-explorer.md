---
id: 3392
title: Chart New Territory with Parse Explorer
date: 2015-03-25T10:50:14+00:00
author: ChristineYen
layout: post
guid: http://blog.parse.com/?p=3392
permalink: /learn/chart-new-territory-with-parse-explorer/
post_format:
  - feat-image
feature_image_style:
  - small
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3687484146"
image: /wp-content/uploads/2015/03/explorer-blog.jpg
categories:
  - Announcements
  - Engineering
  - Learn
tags:
  - analytics
  - queries
---
Today we're excited to announce a new, powerful debugging tool for production issues in your Parse app: the Parse Explorer.

For a large app, it can be complicated to track down when something is going wrong. With thousands or millions of API requests, how do you fix it when a small fraction of these are broken? How do you improve it when some section of them are running slowly?

The Parse Explorer offers a powerful query language to let you investigate what's happening with a subset of your app's API requests. You can use this powerful query language to dig through timestamped, heavily denormalized logs tracking each bit of communication that your app does with Parse. This approach is powerful, yet simultaneously easy to use.

We're also surfacing something we've never explicitly exposed before: the measured latency on each of your requests. We start counting immediately upon receiving your API request and stop counting right before we return a response, so you know just how long your app was waiting for Parse to do its work. We think it's a pretty powerful way to start drilling down on which actions are most costly for your users.

Here's an example. Say our Parse app is growing and we want to figure out what part of the app the increased usage is coming from. Here's where we start:

[<img class="aligncenter size-full wp-image-2750" src="{{ site.url }}/assets/wp-content/uploads/2015/03/explorerhero.png" alt="Explorer Hero" width="1024" />]({{ site.url }}/assets/wp-content/uploads/2015/03/explorerhero.png)

We don't know what sort of data we're looking for yet, so let's pick the most flexible format choice: a table.

We'll start off by looking at the pure number of requests, grouping by time to confirm that this usage is growing. We'll also group by request type hitting Parse to confirm that the distribution of requests is what I'd expect of my app.

[<img class="aligncenter size-full wp-image-2747" src="{{ site.url }}/assets/wp-content/uploads/2015/03/groubyday-masked-dates.png" alt="Explorer Group By Day" width="1024" />]({{ site.url }}/assets/wp-content/uploads/2015/03/groubyday-masked-dates.png)

Sorting by the Count of rows tells us that — yep, the last few days have all been pretty active, and the most common type of request that has recently taken place is an update. I wonder if we can figure out which classes are receiving those updates — and from there, why.

[<img class="aligncenter size-full wp-image-2746" src="{{ site.url }}/assets/wp-content/uploads/2015/03/whichclasses.png" alt="Explorer Which Classes" width="1024" />]({{ site.url }}/assets/wp-content/uploads/2015/03/whichclasses.png)

Either my app is going through crazy growth, or there's something funky going on with extra _Installation updates hitting Parse — and if we keep digging in, we'll discover more and more. Check out the Parse Explorer for your app and [let us know what you think](https://groups.google.com/group/parse-developers)! Access will be rolled out gradually to accounts on [Parse.com](http://Parse.com), so keep an eye out and think about what you'd like to see next. Happy exploring!