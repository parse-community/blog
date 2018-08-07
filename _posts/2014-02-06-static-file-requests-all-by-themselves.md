---
id: 2073
title: Static File Requests, All by Themselves
date: 2014-02-06T18:31:20+00:00
author: christineyen
layout: post
guid: http://blog.parseplatform.org/?p=2073
permalink: /announcements/static-file-requests-all-by-themselves/
dsq_thread_id:
  - "3690488046"
categories:
  - Announcements
  - Engineering
---
It's always been easy to store a static file on Parse, whether through Parse Files or static assets on Parse Hosting. What we haven't made easy, though, is knowing what percentage of your traffic is from your app requesting a static file instead of making an intelligent request to our servers.

We've traditionally lumped requests for those assets into your overall request usage numbers, visible on your dashboard and used for billing:

[<img src="{{ site.url }}/assets/wp-content/uploads/2014/02/requests.png" alt="File Requests as part of Dashboard Requests" width="972" height="289" class="aligncenter size-full wp-image-2076" />]({{ site.url }}/assets/wp-content/uploads/2014/02/requests.png)

... but only Parse Hosting static file requests were reflected in our analytics graphs.

We're happy to say that, as of last week, we've cleaned the situation up a bit. File Requests are still reflected in your dashboard usage numbers (and used for billing: "Requests" reflect the sum of API Requests and File Requests), but the two are now separated out in analytics graphs.

The default "API Requests" option will only reflect requests that hit our servers (e.g. an object save, user login, etc) while the new "File Requests" option will reflect Parse File and Parse Hosting file requests:

[<img src="{{ site.url }}/assets/wp-content/uploads/2014/02/filerequests.png" alt="File Requests in Analytics Graphs" width="645" height="264" class="aligncenter size-full wp-image-2077" />]({{ site.url }}/assets/wp-content/uploads/2014/02/filerequests.png)

We've been working busily on giving you clearer, more reliable insight into usage of your Parse apps. Let us know what you think, and keep an eye out for more goodies coming your way!