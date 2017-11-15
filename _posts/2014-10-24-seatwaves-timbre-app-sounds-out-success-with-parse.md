---
id: 2581
title: "Seatwave's Timbre App Sounds Out Success with Parse"
date: 2014-10-24T21:40:07+00:00
author: nancyxiao
layout: post
guid: http://blog.parseplatform.org/?p=2581
permalink: /customers/seatwaves-timbre-app-sounds-out-success-with-parse/
post_format:
  - basic
dsq_thread_id:
  - "3685596222"
categories:
  - Customers
---
Widely hailed by its users with a five-star rating in the App Store, Timbre is a welcome addition to any music listener's event-planning experience.  Timbre provides a comprehensive list of music events happening in the U.S. and in the U.K., and greatly simplifies the attendance experience.  Users can create playlists from gigs and receive relevant notifications when bands they follow are touring near their location.  Plus, ticket purchasing becomes much more seamless through Timbre--fans are instantly connected with primary and secondary ticketing providers.
  
[<img class="alignnone size-full wp-image-2583" style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2014/10/timbre.png" alt="Timbre" width="300" height="466" />]({{ site.url }}/assets/wp-content/uploads/2014/10/timbre.png)

Timbre began as a hackathon project in 2012.  By 2013, it was acquired by <a href="http://www.seatwave.com" target="_blank">Seatwave</a>, a UK-based secondary ticketing exchange company.  Today, Timbre's visually captivating experience is available on the App Store, with plans for an Android release in the future.  In addition to the Timbre app, the team has built Timbre Afterparty, a web app focused on post-event sharing and celebration.  Users can upload images, review gigs, and share with friends.  Additionally, Timbre Afterparty provides the setlist for an event, and then allows the user to replay the setlist through a Spotify plugin.

Given the high complexity of pulling information on so many different artists, venues, and shows and linking them with user preferences, Timbre turned to Parse as their backend solution.  <a href="https://parse.com/products/core" target="_blank">Parse Core</a> is used to store user preferences, artist images, reviews, and more.  To engage with fans, <a href="https://parse.com/products/push" target="_blank">Parse Push</a> has proved exceptionally helpful.  Users' music choices, including artists, venues, and events, are stored in Parse.  On a periodic daily basis, Parse is then queried to aggregate event information that is relevant to the user.  This information is then sent in a push notification to a music fan, connecting them to what is sure to be a great musical adventure.

For Timbre's development team, Parse proved to be useful in a variety of ways:

> Our favorite aspect of using Parse is its capability for rapid prototyping with small datasets.  Using Parse saved us hosting costs, and I would recommend it to teams that are looking to prototype quickly.

Download Timbre on the <a href="https://itunes.apple.com/gb/app/timbre-concerts-gigs-live/id533493750?mt=8" target="_blank">App Store</a> today, and never miss out a great show again.