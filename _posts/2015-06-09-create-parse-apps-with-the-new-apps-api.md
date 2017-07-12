---
id: 3601
title: Create Parse Apps with the new Apps API
date: 2015-06-09T15:34:19+00:00
author: Jamie Karraker
layout: post
guid: http://blog.parse.com/?p=3601
permalink: /announcements/create-parse-apps-with-the-new-apps-api/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3835570895"
categories:
  - Announcements
tags:
  - APIs
---
Just two weeks ago, we extended the suite of Parse tools accessible through our REST API when we [released the new Schema API](http://blog.parse.com/announcements/releasing-the-schema-api/). This week, we're going to let the good times keep rolling with the release of the [new Apps API](https://parse.com/docs/rest/guide#apps). The Apps API lets you access your Parse apps and their keys, create new apps, and update your app's settings through our API.

Like the Schema API, the Apps API lets you access information and perform actions programmatically that previously you could only do through your Parse dashboard. You can now create test apps, build your own admin system for all of your apps, or update your app's name, all via our API. Combining this with the Schema API, you could now completely replicate the structure of your existing app into a new test app, easily and programmatically.

Simply send a `GET`, `POST`, or `PUT` request to `https://api.parse.com/1/apps`, along with your email and password for authentication, to access this powerful new API. To learn more about how the API works, check out the [documentation](https://parse.com/docs/rest/guide#apps). As always, let us know what you think of this new API [here](https://parse.com/help), and keep an eye on the [blog](http://blog.parse.com/) for more exciting new APIs coming soon!