---
id: 2486
title: Announcing Parse Analytics Integration with App Links
date: 2014-08-21T17:00:22+00:00
author: ChristineAbernathy
layout: post
guid: http://blog.parse.com/?p=2486
permalink: /announcements/announcing-parse-analytics-integration-with-app-links/
post_format:
  - basic
dsq_thread_id:
  - "3697315659"
categories:
  - Announcements
---
After the initial release of App Links support in the <a href="https://github.com/BoltsFramework/" target="_blank">Bolts SDK</a>, we've been looking for ways to enhance this support and easily provide actionable data to developers. With that in mind, we are pleased to announce Parse support for the App Links analytics hooks that are now available in the Bolts framework.

This Parse support takes App Links events and translates them into Parse Analytics custom events. You can then go to your app's Analytics dashboard to view detailed App Links data--for example, how often an outbound App Links has been clicked or how many times your app has been opened from another App Links-enabled app.

This new open source, lightweight library is available for iOS and Android. You can simply download the <a href="https://github.com/ParsePlatform/ParseAppLinksAnalytics/releases" target="_blank">jar</a> or <a href="https://github.com/ParsePlatform/ParseAppLinksAnalytics/releases" target="_blank">framework</a> file and drop it into your project that's already using the <a href="https://github.com/BoltsFramework/" target="_blank">Bolts SDK</a>. You can also download the source directly from <a href="https://github.com/ParsePlatform/ParseAppLinksAnalytics" target="_blank">GitHub</a>. Then you can start measuring App Links events on Android as follows:

`ParseAppLinksAnalytics.enableTracking(context);`

To measure App Links events in iOS, add the following:

`[PAAnalytics enableTracking];`

You can then check out the custom events area in the Analytics dashboard and look for the following data related to your App Links usage:

`AppLinksInbound`: whenever traffic resulting from your App Links integration opens up your app.
  
`AppLinksOutbound`: whenever your app opens another app via an App Links integration.
  
`AppLinksReturning`: _(iOS only)_ whenever an App Links integrated app returns the user back to your app after a previous outbound link.

Note that these events are only triggered if the App Links are constructed using the <a href="https://github.com/BoltsFramework/" target="_blank">Bolts SDK</a>.

For more information, please see the platform-specific README files on <a href="https://github.com/ParsePlatform/ParseAppLinksAnalytics" target="_blank">GitHub</a>. We'd love to hear your feedback as you use the power of Parse Analytics to help fine tune your app's integration with App Links and Bolts.