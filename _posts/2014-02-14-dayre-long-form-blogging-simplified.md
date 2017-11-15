---
id: 2087
title: 'Dayre: Long-Form Blogging Simplified'
date: 2014-02-14T20:02:52+00:00
author: courtneywitmer
layout: post
guid: http://blog.parseplatform.org/?p=2087
permalink: /non-technical/dayre-long-form-blogging-simplified/
dsq_thread_id:
  - "3688874071"
categories:
  - Customers
  - Non-Technical
tags:
  - APAC
---
[<img style="border: 0pt none; float:right; padding-left:10px; padding-bottom:10px" alt="Dayre on iPhone and Android phones" src="{{ site.url }}/assets/wp-content/uploads/2014/02/phones.png" width="300" height="431" />]({{ site.url }}/assets/wp-content/uploads/2014/02/phones.png)We live in an age of Facebook status updates, Twitter’s 140 character limit, and images that only last 10 seconds before they self-destruct. We’re used to consuming rapidly digestible pieces of information as well as sharing our important (and mundane) moments in similar format. Observing this trend, FTW Tech, the makers of Parse-powered <a href="https://itunes.apple.com/sg/app/dayre/id724596057?mt=8" target="_blank">Dayre</a> (pronounced “diary”) developed a blogging app that compiles multiple short updates per day into a daily blog post, complete with photos, stickers, and other unique touches.

Traditionally working in web applications, Dayre is the company’s second foray into mobile apps. Developed after observing that long-form blogging was taking a hit, co-founder Ming says the idea for Dayre grew from the idea that people have become used to updating in bite-sized pieces. He felt that, “if I boke down each update so it mimicked current trends, and “stitched” these updates together within a day, it would still tell a “story”. Dayre is about telling a story instead of pushing out fragmented updates that lack context and depth.”

Dayre uses Parse to the fullest, incorporating Data, Social, Push, and Cloud Code after the company’s Technical Director, Timothy Teoh, recommended Parse to the development team.

Ming says that nearly all Dayre’s digital assets are stored on Parse, including images, videos, and thumbnails. Cloud Code is used for most functions, as it reduces the amount of code that needs to be written for iOS and Android. They also use it for scheduled jobs that are not time-sensitive, like calculating follower counts.

The app uses Parse's built-in Facebook integration, including email confirmation and authentication libraries, which simplifies these tasks from a development perspective. Users are then notified via push notification when they have new blog followers, when they are mentioned in a post or comment, or when a blog post receives a comment. The company then also uses the simple Parse interface to make general announcements through the Push Console when necessary.

According to Ming, “Parse lowers development costs and time significantly. The Free plan is all you need for development before switching to Pro at launch and scaling up to Enterprise if the product takes off (like ours did!) or back to Free if it doesn't. Parse allowed us to focus solely on app development and design, rather than worrying about server and database administration. In the growth phase it also allowed us to eliminate infrastructure or server configuration as a factor when debugging or deciding how to scale out Dayre.”

The app has taken off in East Asia, particularly in Singapore and Malaysia, but is available worldwide on <a href="https://play.google.com/store/apps/details?id=com.ftwtech.dayreme" target="_blank">Android</a> and <a href="https://itunes.apple.com/sg/app/dayre/id724596057?mt=8" target="_blank">iOS</a>. You can read a review of the app from the Next Web <a href="http://thenextweb.com/apps/2013/11/22/dayre-could-be-the-hassle-free-solution-to-long-form-blogging-youre-looking-for/#!tBZLR" target="_blank">here</a> and learn more about using Dayre for your own “diary” via <a href="http://www.makeuseof.com/tag/blog-on-the-go-with-ios-android-app-dayre/" target="_blank">this tutorial from Make Use Of</a>.