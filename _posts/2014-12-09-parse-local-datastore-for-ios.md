---
id: 2646
title: Parse Local Datastore for iOS
date: 2014-12-09T15:51:44+00:00
author: Grantland Chew
layout: post
guid: http://blog.parse.com/?p=2646
permalink: /learn/parse-local-datastore-for-ios/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3607802111"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
image: /wp-content/uploads/2014/12/local-datastore-for-ios-blog-header-1024x417.jpg
categories:
  - Announcements
  - Engineering
  - Learn
tags:
  - iOS
---
Too often, we've seen bad reception or lack of connectivity become the downfall of what could have been an incredible user experience. Many mobile apps are simple clients that display data straight from a server, losing all functionality without an internet connection. Because of this, people who use these apps face painful loading screens and broken features. The developers who build these apps want a better experience, but it's not easy to build all the underlying logic to handle local data storage gracefully.

Parse Local Datastore makes it a lot easier, and today we're excited to announce that it's available in our iOS/OS X SDK. You can now create a seamless, uninterrupted user experience free from connectivity constraints.

It's the same functionality found in [our Android implementation](http://blog.parse.com/2014/04/30/take-your-app-offline-with-parse-local-datastore/), but now for iPhones , iPads, and Macs everywhere. `[PFObject pin]` persists `PFObject`s to the Local Datastore and `[PFObject unpin]` removes them. Once pinned, you can access them anytime with a normal `PFQuery`:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">PFQuery *query = [PFQuery queryWithClassName:@"Feed"];

// Pin PFQuery results
NSArray *objects = [query findObjects]; // Online PFQuery results
[PFObject pinAllObjectsInBackground:objects];

...

// Query the Local Datastore
[query fromLocalDatastore];
[query whereKey:@"starred" equalTo:@YES];
[[query findInBackground] continueWithBlock:^id(BFTask *task) {
// Update the UI
}]];</pre>

Pinned `PFObject`s will remain accessible while offline — through app exits and restarts — until they are unpinned. Subsequent saves, updates, or deletes will keep your Parse Local Datastore results up-to-date.

You can also choose to update your object using the `PFObject` method `saveEventually`. This will first update its instance in the Local Datastore and then do so remotely, so any changes can be kept up-to-date while offline:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">feedItem.starred = YES;

// No network connectivity
[feedItem saveEventually];

...

PFQuery *query = [PFQuery queryWithClassName:@"Feed"];
[query fromLocalDatastore];
[query whereKey:@"starred" equalTo:@YES];
[[query findInBackground] continueWithBlock:^id(BFTask *task) {
// "feedItem" PFObject will be returned in the list of results
}]</pre>

This also means that if you're already using the `PFObject` method `saveEventually` to protect your app against network failures, you'll get most of this without any additional work--simply by updating to the latest iOS/OSX SDK and enabling Local Datastore.

With Parse, it's easy to take your app offline! Check out our [guide](https://parse.com/docs/ios_guide#localdatastore) and [API docs](https://parse.com/docs/ios/api/) to get started now.