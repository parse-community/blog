---
id: 3609
title: Configure Cloud Code Webhooks with the New Hooks API
date: 2015-06-16T13:51:14+00:00
author: Pavan Athivarapu
layout: post
guid: http://blog.parse.com/?p=3609
permalink: /announcements/configure-cloud-code-webhooks-with-the-new-hooks-api/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3854130529"
categories:
  - Announcements
tags:
  - APIs
  - cloudcode
---
We announced [Cloud Code Webhooks](http://blog.parse.com/announcements/introducing-cloud-code-webhooks/) earlier this year to allow you to manipulate your data using any language or framework of your choice, with code running on your servers. Today, we're happy to introduce a new API that makes it even easier to take advantage of Cloud Code Webhooks.

The new [Hooks API](https://parse.com/docs/rest/guide#hooks) lets you access information and perform actions programmatically that previously you could only do through your Parse dashboard. You can create new webhooks and modify, delete or list the existing webhooks — all quickly and easily via the Hooks API.

Creating a new function webhook is as simple as:

<pre class="line-numbers"><code class="language-bash">curl -X POST \
-H "X-Parse-Application-Id: ${APPLICATION_ID}" \
-H "X-Parse-Master-Key: ${MASTER_KEY}" \ 
-H "Content-Type: application/json" \ 
-d '{"functionName":"sendMessage","url":"https://api.example.com/sendMessage"}' \
https://api.parse.com/1/hooks/functions</code></pre>

This API opens up several new functionalities to the Parse ecosystem. For instance, it enables you to test your Cloud Code in a local environment before deploying it. You can run your code locally and use a tunneling service like [ngrok](https://ngrok.com/) or [ultrahook](http://www.ultrahook.com/) to map your local http endpoints to public URLs. Then, using the Hooks API, you can dynamically create or modify webhooks (for your app) to be served by your test environment.

The Hooks API comes on the heels of several new APIs we've recently released to make app development even easier — check out the new [Apps API](http://blog.parse.com/announcements/create-parse-apps-with-the-new-apps-api/) and the [Schema API](http://blog.parse.com/announcements/releasing-the-schema-api/), too.

To learn more about how the Hooks API works, check out the [documentation](https://parse.com/docs/rest/guide#hooks), and if you have any questions, please reach out to us [here](https://parse.com/help). We're looking forward to seeing what you create!