---
id: 4014
title: A Parse Shutdown Reminder
date: 2017-01-17T11:00:38+00:00
author: kevinlacker
layout: post
guid: http://blog.parseplatform.org/?p=4014
permalink: /announcements/a-parse-shutdown-reminder/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Announcements
---
As [we previously shared](http://blog.parseplatform.org/announcements/moving-on/), the Parse service is shutting down at the end of this month. Specifically, we will disable the Parse service on Monday, January 30, 2017. Throughout the day we will be disabling the Parse API on an app-by-app basis. When your app is disabled, you will not be able to access the data browser or export any data, and your applications will no longer be able to access the Parse API.

If you still have data on Parse that you would like to save, you should export your Parse data as soon as possible. See our [database migration guide](https://parse.com/migration#database) for more information on how to do this. If you need to run your own open-source Parse server, see [the Parse Server documentation](https://github.com/ParsePlatform/parse-server) for help.

Our migration documentation should cover the vast majority of cases. Our goal is for this service shutdown to go as smoothly as possible, so if you have a case where migrating your app off Parse is not handled by the normal documentation, please feel free to contact us at <migration@parse.com> and we will try our hardest to help. We're very grateful for the Parse community of talented developers and hope this transition has been as straightforward as possible.