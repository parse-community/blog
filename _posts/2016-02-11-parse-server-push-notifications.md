---
id: 3923
title: Parse Server Push Notifications
date: 2016-02-11T10:50:34+00:00
author: mengyanwang
layout: post
guid: http://blog.parseplatform.org/?p=3923
permalink: /announcements/parse-server-push-notifications/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Announcements
---
[Parse Server](https://github.com/ParsePlatform/parse-server), the open source Parse backend, now supports [sending iOS and Android push notifications](https://github.com/ParsePlatform/parse-server/wiki/Push). We've been listening closely to your feedback, and push was definitely one of the most requested features.

After setup, you'll be able to send transactional push notifications based on the properties in the Installation class, just like with the Parse.com hosted backend. We've designed the API to conform as closely as possible to the original, including the ability to send to channels and also specific installations based on a query. If you're migrating an existing Parse app with push, Parse Server will get you up and running quickly.

For example, let's say you want to target a push to all people who are using iOS devices and are a fan of the San Francisco Giants; you would send a request like so:

<pre class="line-numbers"><code class="language-bash">
curl -X POST \
     -H "X-Parse-Application-Id: YOUR_APP_ID" \
     -H "X-Parse-Master-Key: YOUR_MASTER_KEY" \
     -H "Content-Type: application/json" \
     -d '{ 
           "where": { 
             "deviceType": { 
               "$in": [ 
                 "ios" 
               ] 
             }, 
             "fan": "Giants"
           }, 
           "data": { 
             "title": "A special discount for Giants fans", 
             "alert": "Check out our app for a 15% discount!"
           } 
         }' \
http://www.myawesomeparseserver.com/parse/push
</code></pre>

We're also introducing [PushAdapter](https://github.com/ParsePlatform/parse-server/wiki/Push#push-adapter), which lets Parse Server send push notifications using any push provider you want. PushAdapter abstracts the way pushes are sent so that you can easily connect it to any service that exposes an API for sending. If you're interested in adding support for other providers, simply [send us a pull request](https://github.com/ParsePlatform/parse-server).

Get started by pulling the latest Parse Server code and checking out [our wiki](https://github.com/ParsePlatform/parse-server/wiki/Push). If you're interested in helping make Parse Server Push Notifications better for everyone, we'd love to work with you.