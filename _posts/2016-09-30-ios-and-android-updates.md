---
id: 4005
title: iOS and Android Updates
date: 2016-09-30T16:00:32+00:00
author: Eric Nakagawa
layout: post
guid: http://blog.parse.com/?p=4005
permalink: /announcements/ios-and-android-updates/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - large
categories:
  - Announcements
---
# iOS and Android Updates

New versions of iOS and Android OS are now widely available. Here are some suggestions to think about as you update your application.

# iOS 10

iOS 10 was released on September 13th. Two additional updates were released since then (10.0.1 and 10.0.2). By now one can assume that most users who would upgrade have already done so.

**Support for devices that connect with 30-pin connectors are gone.** This means your focus should now be on the newest Apple products. This could mean simply adding high resolution images and removing support for older devices in your newest builds.

**Push alerts no longer go away when a user logs in with Touch ID.** This means your push alerts will have more time to engage your users.

**Today widgets now have a new home.** Users can get to the Today screen by swiping left from the lock screen. Adding a Today widget to your app could make it easier for your users to view information from your application.

**You can now integrate your application with Siri.** If your users could benefit from your app being controlled hands-free, then this could be a great addition.

# Android Nougat

Android's latest OS code named “Nougat” began rolling out to devices on August 22.

**Support for multi-view added.** If your app can benefit from running alongside other apps, now's the time to add support.

**Notification direct reply added to let users interact directly from notifications.** If users use your app for messaging each other, then adding support for direct reply could keep them engaged by allowing them to reply from a notification.

**Notifications now bundled.** If you depend on notifications for bringing back users to your app, be sure to see how Nougat's bundling effects readability and open rates.

**Users have more control over notification.** With Nougat users can now quickly silence your app's notifications. Be sure to review how and when you notify your users. Are they gaining value from each notification? If not, you may want to lower the frequency of your notifications or bundle them together.

* * *

We hope some of the tips and suggestions above can help you to improve the experience for your apps across each platform

With these new OS updates it's important to strive to provide the best user experience you can for your users. The Parse SDKs shall continue functioning normally as expected. However, if you have not migrated your application yet, now is an excellent time to familiarize yourself with our [migration guide](https://www.parse.com/migration). Partner migration guides can be found [here](http://blog.parse.com/guides-discounts-and-events/).

Migrating your database is a quick and painlessly process. Take a look at our guide to find a database host like mLab, Object Rocket, or host your database yourself on AWS, Digital Ocean, or another hosting provider.

Good luck migrating!