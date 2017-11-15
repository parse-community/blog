---
id: 3707
title: 'Improving Push: Better Error Reporting, Localization, and Reusable Audiences'
date: 2015-08-19T13:13:34+00:00
author: vítorfernandes
layout: post
guid: http://blog.parseplatform.org/?p=3707
permalink: /announcements/improving-push-better-error-reporting-localization-and-reusable-audiences/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - small
dsq_thread_id:
  - "4048317892"
image: /wp-content/uploads/2015/08/push-updates1.jpg
categories:
  - Announcements
tags:
  - localization
  - push
  - updates
---
[Parse Push](https://parse.com/products/push) is one of our most widely used products. In fact, this year at the F8 developer conference, we [shared](https://developers.facebooklive.com/videos/495) that Parse push notifications reach an average of **150 million recipients every day**. We're always working on ways to make Push even more effective and simpler to implement — and today, we're excited to share some updates.

* * *

## Push Delivery Report

<img class="aligncenter wp-image-3708 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/08/PushErrorReporting.png" alt="Push Delivery Reports UI" width="1023" height="572" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/PushErrorReporting.png 1023w, {{ site.url }}/assets/wp-content/uploads/2015/08/PushErrorReporting-300x168.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/PushErrorReporting-875x489.png 875w" sizes="(max-width: 1023px) 100vw, 1023px" />

Several factors can cause a push to fail: Maybe the certificates have expired, maybe the installations contain incorrect data, or maybe the user just uninstalled the app. Knowing the reason for push errors is the first step to finding a solution — so today, we're launching the Push Delivery Report to help you more **easily access the information you need to diagnose push errors**.

Now, you can verify the reason why each of your notifications didn't reach the intended recipient, and use that information to work toward improving engagement in your app. Data is collected in real-time, so errors are accounted and should be available on the push details page as soon as the push networks answer with a failure.

* * *

## Push Localization

<img class="alignnone size-full wp-image-3728" src="{{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-11.28.03-AM.png" alt="" width="990" height="676" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-11.28.03-AM.png 990w, {{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-11.28.03-AM-300x205.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-11.28.03-AM-875x597.png 875w" sizes="(max-width: 990px) 100vw, 990px" />

For any app with a global user base, it's crucial to localize content. This makes your app accessible to more people, and thus drives growth. One study from the app analytics company Distimo showed that localizing iPhone application text resulted in significantly more downloads — an increase of 128% per country, in fact (Source:"[Localized But Not Local](http://developers.apps.opera.com/images/uploads/pdfs/insights-02-localized-but-not-local.pdf)" ). Now, you can get the benefits of localization in your Parse push notifications with a new feature called Push Localization.

Push Localization gives you an **easy way to compose localized push notifications through our web push console**. Many developers have requested this feature, and some have even implemented their own language detection. Now, all Parse mobile SDKs (iOS, Android and Windows Phone) will detect and store the user's language in the installation object. All you need to do to start taking advantage of this feature is to upgrade to the latest Parse SDK. Be sure to check out the [Push Localization section](https://parse.com/docs/ios/guide#push-notifications-push-localization) in our guides for more details.

* * *

## Saved Audiences

<img class="alignnone size-full wp-image-3716" src="{{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-8.00.43-AM.png" alt="Screen Shot 2015-08-19 at 8.00.43 AM" width="1194" height="1154" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-8.00.43-AM.png 1194w, {{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-8.00.43-AM-300x290.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-8.00.43-AM-1024x990.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/08/Screen-Shot-2015-08-19-at-8.00.43-AM-875x846.png 875w" sizes="(max-width: 1194px) 100vw, 1194px" />

For developers who use Parse Push, Custom Segments is a powerful way to send push notifications to specific groups of people. Starting today, you can use the new Saved Audiences feature to **save your frequently used audiences**, so you can easily push multiple notifications to the same category of people. Check it out in the push console.

* * *

We hope these updates help make Parse Push even more powerful and easy to use. As always, let us know your feedback!