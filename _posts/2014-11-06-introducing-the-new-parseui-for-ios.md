---
id: 2599
title: Introducing the new ParseUI for iOS
date: 2014-11-06T21:00:03+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parseplatform.org/?p=2599
permalink: /announcements/introducing-the-new-parseui-for-ios/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3612820123"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
image: /wp-content/uploads/2014/11/parseui-hero.png
categories:
  - Announcements
---
A couple of months ago, iOS added new screen resolutions for developers to support in their apps.  As a result, many developers needed to update their user interfaces to look native on these new screen sizes.

Today we're introducing ParseUI for iOS, a collection of user interface components aimed to streamline and simplify user authentication, data list display, and other common app elements. You might be familiar with these components already, as they were previously packaged inside the main Parse iOS SDK. With this release, we've updated how they look and made them resolution-independent to ensure a polished presentation on any screen.

## A New Look

Inside this release you'll find all new designs for every component with simplified user interfaces plus many under-the-hood improvements, such as smoother animations and faster rendering. To give you an example, here's how \`PFLogInViewController\` looked before and how it looks today:

<div style="text-align: center;margin-top: 20px;margin-bottom: 30px">
  <img class="aligncenter wp-image-2601" src="{{ site.url }}/assets/wp-content/uploads/2014/11/parseui-ios-screenshot-1.png" alt="ParseUI iOS New Look" width="640" height="520" />
</div>

## Resolution-Independent

All components were rebuilt from scratch to be resolution-independent, meaning they both look great and are native on any screen resolution. This resolution-independent approach also introduces support for more presentation options. It gives you the flexibility to comprehensively customize everything within your application's navigation and layout.

<div style="text-align: center;margin-top: 20px;margin-bottom: 30px">
  <img class="aligncenter wp-image-2600" src="{{ site.url }}/assets/wp-content/uploads/2014/11/parseui-ios-screenshot-2.png" alt="ParseUI iOS" width="640" height="480" />
</div>

## Open Source

ParseUI is all open source, and you can view the <a title="ParseUI on GitHub" href="https://github.com/ParsePlatform/ParseUI-iOS" target="_blank">code on GitHub</a>. You can also access the new version of the Parse iOS SDK <a title="Parse SDK for iOS" href="https://parse.com/docs/downloads/" target="_blank">here</a>.

We're really excited to be open sourcing more and more of our product. Check out these other projects on GitHub: <a title="BoltsFramework" href="https://github.com/BoltsFramework/" target="_blank">BoltsFramework</a>, <a title="Parse PHP SDK" href="https://github.com/parseplatform/parse-php-sdk" target="_blank">Parse PHP SDK</a>, <a title="ParseUI for Android" href="https://github.com/ParsePlatform/ParseUI-Android" target="_blank">ParseUI for Android</a> and <a title="Parse App Links Analytics" href="https://github.com/ParsePlatform/ParseAppLinksAnalytics" target="_blank">App Links Analytics</a>.

We can’t wait to see what you build with it! Send us your feedback!