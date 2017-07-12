---
id: 2640
title: Migrating from Urban Airship
date: 2014-12-08T19:15:11+00:00
author: Mattieu Gamache-Asselin
layout: post
guid: http://blog.parse.com/?p=2640
permalink: /learn/migrating-from-urban-airship/
post_format:
  - basic
dsq_thread_id:
  - "3607801252"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Engineering
  - Learn
  - Tutorial
tags:
  - migration
  - push
---
Making mobile development as easy as possible has always been our mission at Parse. And ensuring your data is portable and remains yours is an important part of that mission. We've worked hard on improving the accessibility of your data on Parse and are committed to ensuring our customers can access their data at any time.

In that spirit, we also want to make sure new customers are able to easily migrate from other providers to Parse. We noticed that the push notification provider Urban Airship [recently made their product paid only](http://urbanairship.com/blog/2014/06/26/changes-coming-to-urban-airships-free-developer-edition "UA paid only"). So, a few of our engineers spent a weekend building a [very simple migration tool](https://parse.com/migrate/urban_airship "Parse Parachute"). This tool will import all of your Urban Airship push subscriptions for iOS and Android (GCM) devices to Parse. Simply enter your Urban Airship app key and secret and we'll take care of the rest!

Parse Push allows developers to send **unlimited** push notifications to up to 1 million devices for free every single month. Beyond that, the pricing is simple: you'll pay $0.05 a month for every 1000 extra devices. That's $50 for another million devices, and you can send them as many pushes as you want.

[<img class="aligncenter size-full wp-image-2641" style="margin: auto; display: block;" src="{{ site.url }}/assets/wp-content/uploads/2014/12/image.png" alt="image" width="800" />]({{ site.url }}/assets/wp-content/uploads/2014/12/image.png)

After the import is complete, check out the Data Browser to see all of your data. You can then immediately start sending pushes to existing devices without requiring an update to your app. Until you require an update, you can continue using the migration tool to import any new devices. Don't worry, we're keeping track of which ones you've already imported, so there won't be any duplicates. Once you've integrated the Parse SDK and updated your app, new devices will start showing up in Parse. You can then start taking advantage of our powerful analytics features. To learn more about the steps of migrating to Parse, take a look at [our tutorial](https://www.parse.com/tutorials/migrating-from-urban-airship) that will guide you through the whole process.

We're excited for you to experience all that Parse has to offer. With Parse Push, you can now enjoy device targeting, in-depth analytics, and timezone and badge increment support for your push messaging. Plus, don't miss our [brand new A/B testing feature](http://blog.parse.com/2014/11/03/parse-push-experiments-re-engage-more-powerfully-and-more-creatively-with-ab-testing-2/ "Push A/B testing") — engage with your users more intimately and effectively than ever before.

If there's any other importer you'd like to see, [let us know](https://groups.google.com/forum/#!forum/parse-developers "Parse Help")!