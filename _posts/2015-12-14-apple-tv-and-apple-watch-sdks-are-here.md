---
id: 3892
title: Apple TV and Apple Watch SDKs are Here
date: 2015-12-14T10:00:41+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parseplatform.org/?p=3892
permalink: /announcements/apple-tv-and-apple-watch-sdks-are-here/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - small
dsq_thread_id:
  - "4403588446"
image: /wp-content/uploads/2015/02/Parse-watchOS-tvOS-1.png
categories:
  - Announcements
tags:
  - apple
  - SDKs
  - tvOS
  - watchOS
---
With the launch of Apple TV this past fall, a new development medium and platform has been unleashed. Here at Parse, we're extremely excited about the potential of this new platform and its ability to change the way you interact with your television. This isn't the first time we've seen Apple monumentally influence the world of software development—we've also seen its impact with watchOS, iOS, and and OS X. Over the years, our Parse iOS and OS X SDKs have been well-loved by developers around the globe, and in April, [we released](http://blog.parseplatform.org/announcements/introducing-local-data-sharing-for-apple-watch-and-app-extensions/) support for the Apple Watch and App Extensions.

Today, we are happy to announce the launch of new Parse SDKs for Apple's tvOS and watchOS 2. Now, you can utilize the full power and simplicity of the Parse SDKs to build incredibly immersive apps for the Apple TV and the Apple Watch.

* * *

Our primary goal has been to build SDKs that are familiar to anyone using our existing iOS or OS X SDKs, while **making it easier than ever to build native experiences for tvOS and watchOS 2**. Over the course of building this support, the team has faced intriguing new challenges.

## tvOS

One of the biggest challenges that we faced with tvOS was local data storage. Before, in our mobile SDKs, we were treating on-device storage as persistent, meaning it won't be purged by the system.

This local storage was used for things like current users' sessions, as well as current app installation information. On tvOS, however, every file on disk should be treated as a cache or as a temporary storage and could be removed by the system if the app is not running. To utilize the platform at its best and to benefit from existing functionality in our iOS and OS X SDKs, we re-architected one of the core components of local data persistence in a way that still is native to tvOS, but could be used to persist your users' data locally.

Another core difference of tvOS is the user input experience. Since there is no hardware keyboard or big touchscreen, inputting text feels challenging and less immersive. To counter this problem, we updated our <a href="https://github.com/ParsePlatform/ParseFacebookUtils-iOS/releases/latest" target="_blank">Facebook integration library</a> to give you the ability to login to tvOS apps using the recently announced <a href="https://developers.facebook.com/blog/post/2015/11/25/tvOS-SDK-beta/" target="_blank">Facebook tvOS SDK</a>, by using just a few lines of code.

## watchOS 2

Earlier this year, we released support for <a href="http://blog.parseplatform.org/announcements/introducing-local-data-sharing-for-apple-watch-and-app-extensions/" target="_blank">watchOS 1 and App Extensions</a>, which lets you build apps that could have an extension running on Apple Watch. With this new SDK for watchOS 2 we've brought the full amazing experience of the Parse SDK, so you can build apps that run purely on the Watch. As a result, your app can load faster and won't need a phone to stay connected.

One of the most exciting aspects of this project was the development process itself. Throughout it all, the open source community was instrumental in providing the Parse team with early end-to-end testing and valuable feedback, which resulted in an incredibly rapid release. You can read more about our efforts of open sourcing our SDKs [here](http://blog.parseplatform.org/announcements/open-sourcing-our-sdks/).

We are excited to bring the Parse SDK to more platforms than ever, and especially to see what you build for the big screen with Apple TV and for the Apple Watch!

**Ready to dig into the code?**
  
Both <a href="https://github.com/ParsePlatform/Parse-SDK-iOS-OSX/releases/latest" target="_blank">new SDKs</a>, as well as the updated <a href="https://github.com/ParsePlatform/ParseFacebookUtils-iOS/releases/latest" target="_blank">Facebook integration library</a> are available on GitHub.