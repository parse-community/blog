---
id: 2508
title: Announcing Parse Config
date: 2014-09-08T17:38:23+00:00
author: Karan Gajwani
layout: post
guid: http://blog.parse.com/?p=2508
permalink: /announcements/announcing-parse-config/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3685575483"
image: /wp-content/uploads/2014/09/Parse-Config-Blog-Banner.png
categories:
  - Announcements
---
We at the Parse London office are happy to announce Parse Config: the simplest way to store your app's parameters for updating on the fly. Keeping configurations out of your app's binary means config changes do not require a fresh app release.

Say you have a background image in your game, and you would like to change it on the fly during the Christmas season. All you would have to do is swap the image on the Parse Config dashboard. Or, for example, you have this shiny new feature that you haven't released yet, and would like to have a few people dogfood it. You could trickle out these by specifying a “dogfooding” array of user IDs.

<img class="size-full wp-image-2512" src="{{ site.url }}/assets/wp-content/uploads/2014/09/config_editor.png" alt="Parse Config Dashboard" width="994" height="316" />

In the past, some Parse developers have built their own configuration management using a Parse Object, and then modified its columns in the Data Browser. This approach required manually caching this object on mobile devices to avoid waiting for a query to run on every app restart. Parse is all about abstracting away common tasks so we wanted to build something for this use case.

Parse Config makes the experience of handling configuration much smoother than ever before. We updated our iOS, Android, JS, .NET and Unity SDKs to provide intuitive method calls to help sync configurations in the Parse Cloud. Our SDKs automatically cache the last-fetched configuration, so you don't have to re-fetch it after an app restart.

Here is an example snippet of what the Parse Config API looks like. Alternatively, you can have a look at the <a href="https://github.com/ParsePlatform/AnyWall" target="_blank">Anywall source code</a>, which has now been updated to use Parse Config.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[PFConfig getConfigInBackgroundWithBlock:^(PFConfig *config, NSError *error) {
  NSArray *distanceOptions = config[@"searchDistanceOptions"];
  if (!distanceOptions) {
    // No config for distance options - fallback to the default ones
    distanceOptions = @[ @250.0, @1000.0, @2000.0, @5000.0 ];
  }
  self.distanceOptions = distanceOptions;
  [self.tableView reloadData];
}];</pre>

Our simple yet flexible API gives you precise control over when to fetch a new configuration and when to use the last-known configuration.The `PFConfig` is an immutable dictionary that can be used to retrieve configuration parameters. Our SDKs automatically persist the last-known `PFConfig` instance, and can be retrieved with `[PFConfig currentConfig]` so you don't have to worry about losing the last-known configuration if a user decides to close your app.

As you may expect, the fetching of the configuration from the server is an asynchronous call. While the fetch occurs in the background, you can keep using the last-held `PFConfig` instance, without worrying about it suddenly changing underneath you. After the fetch completes, you can decide to switch to the latest `PFConfig` anytime.

We are always looking to improve our products and would love to hear your feedback. <a href="https://parse.com/docs/ios_guide#config/iOS" target="_blank">Check out our guides</a>, try it out and let us know what you think!