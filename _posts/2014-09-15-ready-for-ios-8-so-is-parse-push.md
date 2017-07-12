---
id: 2523
title: Ready for iOS 8? So is Parse Push
date: 2014-09-15T21:17:16+00:00
author: ThomasBouldin
layout: post
guid: http://blog.parse.com/?p=2523
permalink: /learn/ready-for-ios-8-so-is-parse-push/
post_format:
  - basic
dsq_thread_id:
  - "3671176503"
categories:
  - Learn
---
In this year's WWDC, Apple announced some great [changes to iOS Notifications](http://devstreaming.apple.com/videos/wwdc/2014/713xx1il4h4ur9c/713/713_whats_new_in_ios_notifications.pdf?dl=1). First, Apple has increased the maximum payload size to two kilobytes rather than 256 bytes. This new limit is retroactive and works with all existing devices! The more subtle changes to push notifications involve security and interaction with notifications.

"Silent" pushes are push notifications that don't create UI; they instead tell your app to fetch or react to new content available online. In iOS 8, Apple has separated out the permissions for UI and push. The push permission is auto-accepted by default too! This means your iOS 8 apps will be able to much more reliably depend on the ability to receive silent notifications in iOS 8. To migrate your app, change the following code:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">// Before iOS 8:
[[UIApplication sharedApplication] registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert |
         UIRemoteNotificationTypeBadge |
         UIRemoteNotificationTypeSound];</pre>

to

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">// For iOS 8:
UIUserNotificationSettings *settings =
    [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeAlert |
          UIUserNotificationTypeBadge |
          UIUserNotificationTypeSound
          categories:nil];
[[UIApplication sharedApplication] registerUserNotificationSettings:settings];
[[UIApplication sharedApplication] registerForRemoteNotifications];</pre>

Even if the user declines permission for creating UI, the permission to send (silent) pushes is auto-accepted for most users.Notice the `nil` param for `categories` above? Categories unlock a whole new dimension to push notifications in iOS 8. Categories describe "actions" that should be presented in your notification in various views. Actions provide custom buttons your users can use when interacting with your push notification; your action can launch the app into the foreground or even trigger a background action, such as accepting or declining a calendar invite. To enable this feature, you must define a [`UIUserNotificationCategory`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIUserNotificationCategory_class/index.html#//apple_ref/occ/cl/UIUserNotificationCategory). These categories have an `identifier` string and a map of [`UIUserNotificationActionContext`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIUserNotificationCategory_class/index.html#//apple_ref/doc/c_ref/UIUserNotificationActionContext) to many [`UIUserNotificationActions`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIUserNotificationAction_class/index.html). A [`UIUserNotificationActionContext`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIUserNotificationCategory_class/index.html#//apple_ref/doc/c_ref/UIUserNotificationActionContext) comes in two flavors: default and minimal. The default context specifies which actions should be presented when the notification has an alert UI and supports a maximum of four actions; the minimal context specifies which actions should be presented when the notification has a banner UI or is on the lock screen and supports a maximum of two actions.  Once you've registered notification categories with your application, sending them via Parse Push is easy: simply pass the category identifier as the [`category` option](https://parse.com/docs/push_guide#options/iOS) in your push. This feature is supported retroactively on all Parse SDKs, you don't even need to upgrade! You may, however, want to check out the new [`UIApplicationDelegate`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/) method [`application:handleActionWithIdentifier:forRemoteNotification:completionHandler:`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/#//apple_ref/occ/intfm/UIApplicationDelegate/application:handleActionWithIdentifier:forRemoteNotification:completionHandler:)

<img class="size-full wp-image-2524 aligncenter" src="{{ site.url }}/assets/wp-content/uploads/2014/09/upload.png" alt="iOS 8 allows developers to define actions an end user can take when responding to a notification." width="271" height="480" />

We look forward to seeing the great things you build with Parse and iOS 8!