---
id: 2227
title: Production vs Development Apps
date: 2014-03-31T10:00:04+00:00
author: christophetauziet
layout: post
guid: http://blog.parseplatform.org/?p=2227
permalink: /announcements/production-vs-development-apps/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
dsq_thread_id:
  - "3671208337"
image: /wp-content/uploads/2014/03/prodapps3.png
categories:
  - Announcements
  - Engineering
---
It's a very common use-case to have apps built on Parse that are both still in development and already in production. To help you make a clear distinction between Development Apps and Production Apps, we're adding a switch in your app's settings that lets you mark an app as a Production App. This will not only allow you to visually differentiate between your production apps and those still in development mode, but will in the future allow us to add security features that prevent you from accidentally editing content that is in production, or sending test push notifications to actual users ("Whoops!").

We're pretty excited about this, and hope you'll find it useful!