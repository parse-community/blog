---
id: 2647
title: Introducing Parse Crash Reporting
date: 2014-12-09T15:49:52+00:00
author: IslamIsmailov
layout: post
guid: http://blog.parse.com/?p=2647
permalink: /learn/introducing-parse-crash-reporting-2/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3607802547"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
image: /wp-content/uploads/2014/12/crash-blog-header-1024x417.jpg
categories:
  - Engineering
  - Learn
tags:
  - analytics
  - bugs
---
_Update: The Parse Crash Reporting tool is being deprecated in favor of great native solutions from Apple and Google. We will continue supporting Parse Crash Reporting until **<span class="aBn" tabindex="0" data-term="goog_2038453709"><span class="aQJ">March 1, 2016</span></span>**. Find out how you can switch with these <a href="https://developer.apple.com/library/watchos/documentation/IDEs/Conceptual/AppDistributionGuide/AnalyzingCrashReports/AnalyzingCrashReports.html?mkt_tok=3RkMMJWWfF9wsRomrfCcI63Em2iQPJWpsrB0B%2FDC18kX3RUvJr2cfkz6htBZF5s8TM3DVlNDXqlH8UEIQrQ%3D" target="_blank">Apple docs</a> and these <a href="http://developer.android.com/distribute/googleplay/developer-console.html?mkt_tok=3RkMMJWWfF9wsRomrfCcI63Em2iQPJWpsrB0B%2FDC18kX3RUvJr2cfkz6htBZF5s8TM3DVlNDXqlH8UEIQrQ%3D#reviews-reports" target="_blank">Google docs</a>. Questions or concerns can be shared to community@parse.com._

We at the Parse London office are happy to announce Parse Crash Reporting: the simplest way to register, track, and resolve crashes in your apps. Being able to capture and fix crashes efficiently means fewer frustrated users and better retention--one of the most important metrics to any developer.

In the past, we've seen Parse developers use third-party crash reporting tools, but this approach required developers to manage several different SDKs, learn new APIs, and monitor many dashboards at once. We wanted it to be simpler. Part of Parse's core mission has always been to abstract away common tasks and streamline the developer's experience — so we built a way to manage crash reports right within Parse.

[<img class="alignnone size-large wp-image-2649" style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2014/12/crash-reporting-screenshot-1024x558.jpg" alt="Crash Reporting" width="584" height="318" />]({{ site.url }}/assets/wp-content/uploads/2014/12/crash-reporting-screenshot.jpg)

Say you've released a new version of your app that unfortunately "features" a newly introduced bug. Have no fear! All you have to do is enable Parse Crash Reporting, making the experience of handling crashes in your app much smoother than ever before. We've updated our iOS and Android SDKs to provide a simple and intuitive interface that detects the bug, pinpoints the issue, and generates a report allowing you to resolve problems quickly and easily. With this update, our SDKs automatically cache and resend your crash reports if connectivity is spotty. And, our backend tracks bugs on a per-version basis, so that if you've reintroduced an old bug in a new version of your app, Crash Reporting still ensures you'll notice and fix it as soon as possible—even if you've previously marked it as "resolved.”

Here's an example snippet showing how you can enable Parse Crash Reporting for Android:

<pre class="brush: actionscript3; gutter: false">ParseCrashReporting.enable(context);</pre>

Here's an example snippet showing how you can enable Parse Crash Reporting for iOS:

<pre class="brush: actionscript3; gutter: false">[ParseCrashReporting enable];</pre>

As you may have expected, the sending of crash reports from the client is asynchronous and takes place automatically in the background.

We're always looking to improve our products and would love to hear your feedback. <a href="https://parse.com/docs/ios_guide#crashreporting/iOS" target="_blank">Check out our guides</a>, try it out, and let us know what you think!