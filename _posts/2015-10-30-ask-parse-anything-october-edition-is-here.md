---
id: 3866
title: 'Ask Parse Anything - October Edition Is Here!'
date: 2015-10-30T15:04:53+00:00
author: nancyxiao
layout: post
guid: http://blog.parse.com/?p=3866
permalink: /videos/ask-parse-anything-october-edition-is-here/
post_format:
  - feat-video
feature_video:
  - https://youtu.be/QKqRZ90VxKA
video_source:
  - youtube_share
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4275493059"
categories:
  - Learn
  - Videos
tags:
  - apa
  - askparseanything
  - bestpractices
  - cloudcode
---
Our developer advocate Fosco takes on your questions -- plus, don't miss our new segments "Cloud Code Corner (or If I Were You...)" and "Did You Know?" for awesome tips, tricks, and secrets of the trade!

<ol class="standard-list">
  <li>
    How do I secure Parse API keys in mobile apps?
  </li>
  <li>
    Why don't you propose to the community to translate the docs? #French
  </li>
  <li>
    Will objects be able to support multiple geopoints, or the ability to allow "nearest location" queries of objects with relations to multiple geopoints?
  </li>
  <li>
    As a successful tech company dealing with many software engineers and development environments, what tools have you found that work well for you to seamlessly allow for continuous development and flexibility for different teams to work easily?
  </li>
  <li>
    What are the best practices for designing unit tests for apps that use Parse? (e.g. one may need to delete a user generated during the test in the tearDown of the test class)
  </li>
  <li>
    When will you be updating the Parse SDK to support watchOS 2? What do you suggest we do in the meantime?
  </li>
  <li>
    When will you release a sample project with native watchOS 2 integration?
  </li>
  <li>
    Local storage support for the JS SDK: when? Love you guys.
  </li>
  <li>
    The iOS API documentation is written for Objective-C. Is there an equivalent for Swift? If so where can it be found?
  </li>
  <li>
    From my Android code, I send a map of phone numbers into my Cloud Code function. There, I loop over the map of numbers, and use the Twilio module to send an SMS to each phone number. Does this only count as 1 Parse API request?
  </li>
  <li>
    I login to my Parse account using my password that I keep very secret and which gives me full access to my customers database. Is it possible to use two-factor authentication?
  </li>
  <li>
    I need to upload one or more images on Parse, so they are stored as an array. How I can remove an image from an array or add new images to that images array
  </li>
</ol>

**Cloud Code Corner**

<ol class="standard-list">
  <li>
    Any plans on opening up Cloud Code to support node modules?
  </li>
  <li>
    Thanks and congrats on open-sourcing the SDKs! In the vein of improving developer experience, have you considered improving the logging, backup, and testing systems?
  </li>
  <li>
    Are you planning to move from Backbone to React? Lots of the earlier examples seem stale, and debugging code is laborious -- ever try to debug a Cloud function with five or more nested Promises? What's the plan?
  </li>
  <li>
    Is there a way to generate a "reset password" link without sending an email automatically? I'd like to handle the delivery on my own, plus customize the HTML / CSS of the email sent out.
  </li>
  <li>
    In Forgot Password, how can I display a button in an email, instead of the Parse link?
  </li>
  <li>
    How do I setup an Express app in Cloud Code to use the “handlebars” template system instead of EJS?
  </li>
  <li>
    Does Parse plan to implement multi-part form encoding for Parse.Cloud.httpRequest?
  </li>
  <li>
    A growing number of REST APIs require the PATCH verb. Are there any plans to add support for this HTTP verb to Parse.Cloud.httpRequest?
  </li>
</ol>

**Did You Know?**

<ul class="standard-list">
  <li>
    You'll get a beforeSave or beforeDelete event triggered even if the requesting user has no write access to the object (by ACL permissions).
  </li>
  <li>
    With webhooks, you can extend your time constraints from 3 seconds on all database triggers and 15 seconds on cloud functions, to 30 seconds for everything. Plus, you can continue processing after responding to the request.
  </li>
  <li>
    Our iOS SDK uses something called Single-Instance mode. If you have a copy of object X, and somewhere else you issue a query and object X is included in the results, your original instance of object X is updated too.
  </li>
  <li>
    Our team is coming to Europe next month! Keep up-to-date on where you can meet us in person by visiting the Parse blog.
  </li>
</ul>