---
id: 2533
title: Parse SDK for iOS 8, Performance, and Security
date: 2014-09-18T11:00:12+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parse.com/?p=2533
permalink: /announcements/parse-sdk-for-ios-8-performance-and-security/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3666640722"
image: /wp-content/uploads/2014/09/parse-ios8-banner.png
categories:
  - Announcements
---
We're excited that the big day is finally here — iOS 8 is finally available to everyone! We spent the last few weeks waiting patiently and getting ready. Today we're rolling out a new version of the Parse SDK for iOS with support for iOS 8 as well as a bunch of other performance and security improvements.

## Parse and iOS 8

We updated our SDK to make sure it runs smoothly on iOS 8 and benefits from all the new APIs available. Just to name a couple, we've updated how [`[PFGeoPoint geoPointForCurrentLocationInBackground:]`](https://parse.com/docs/ios/api/Classes/PFGeoPoint.html#//api/name/geoPointForCurrentLocationInBackground:) works to be smarter about requesting the appropriate permissions depending on the state of the app, and we've updated our push notification integration everywhere to use the new permissions style and support the category key.

## Performance Improvements for Parse Files

Parse Files let you easily store application files in the cloud that would otherwise be too large or cumbersome to fit into a regular database-style Parse Object. This release of the iOS (and OS X) SDK brings greatly improved performance for Parse Files. Uploading is now up to 3 times faster and downloading is up to 35% faster. We're also using fewer system resources which leads to better battery usage for file-heavy apps.

## Improved Security for Account Data

The Parse SDK now uses the System Keychain on both iOS and OS X to store sensitive user information tied to PFUser. All of this happens automatically and under the hood so you don't need to make any changes on your end.

## Try It Out

The latest iOS SDK is now available for <a href="https://parse.com/docs/downloads/" target="_blank">download here</a>. Send us your feedback! We hope you're as excited as we are about the new features and possibilities that iOS 8 brings.