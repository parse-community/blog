---
id: 2489
title: Open Sourcing the f8 Conference Apps
date: 2014-08-28T18:21:40+00:00
author: Christine Abernathy
layout: post
guid: http://blog.parse.com/?p=2489
permalink: /announcements/open-sourcing-the-f8-conference-apps/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
dsq_thread_id:
  - "3687785069"
image: /wp-content/uploads/2014/08/f8-blogpost.png
categories:
  - Announcements
  - Engineering
---
As part of the f8 conference experience, we launched <a href="http://itunes.apple.com/us/app/f8/id853467066?mt=8" target="_blank">iOS</a> and <a href="http://play.google.com/store/apps/details?id=com.parse.f8" target="_blank">Android</a> apps built entirely on Parse. The app enabled conference attendees to stay up-to-date with upcoming talks, get to know the speakers, and receive push notifications on talks they had marked as favorites.

This app grew out of the <a href="http://blog.parse.com/2013/10/01/parse-developer-day-apps-now-open-sourced/" target="_blank">Parse Developer Day apps</a>, and was thoroughly customized to match the f8 experience. Using Parse to build these apps was a fun experience, and it took one iOS and Android developer around two weeks to deliver the apps. Parse Data was used to store all the conference data and much of the User Interface (UI) customizations and assets, enabling quick iteration on design changes across iOS and Android. Even after the app was shipped, last minute updates were possible without touching code, simply by using the Data Browser. And, as talks were in session, the Push Console was used for ad hoc messaging to conference attendees--it's vitally important to know when to pick up those T-shirts.

Today, we are releasing the source code for these apps so you can peek under the hood and see how they were built. The source code is available on <a href="https://github.com/ParsePlatform/f8DeveloperConferenceApp" target="_blank">GitHub</a>. We hope they will serve as good guidance for how to build apps on Parse. We also hope others find them useful as a starting point for their own conference apps. We can’t wait to see what you do with them!