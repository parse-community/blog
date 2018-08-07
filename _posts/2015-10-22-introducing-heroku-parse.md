---
id: 3857
title: Introducing Heroku + Parse
date: 2015-10-22T10:00:53+00:00
author: pavanathivarapu
layout: post
guid: http://blog.parseplatform.org/?p=3857
permalink: /announcements/introducing-heroku-parse/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4249407612"
image: /wp-content/uploads/2015/10/Heroku-Parse-Blog.jpeg
categories:
  - Announcements
tags:
  - cloudcode
  - heroku
  - node
---
One powerful feature of Parse is [Webhooks](http://blog.parseplatform.org/announcements/introducing-cloud-code-webhooks/), which lets you connect your Parse app to any server on the web to use a custom server environment rather than Parse's hosted Cloud Code. We're always looking for ways to make Parse easier to use, and recently, we [released Node.js middleware](http://blog.parseplatform.org/learn/using-node-js-with-parse/) to make it easier to connect your Parse app to a Node.js server.

Today we're excited to announce that we're partnering with [Heroku](https://www.heroku.com/) to make it even easier to connect your Parse app to Node.js code. If you like Parse's Cloud Code but wish you had a full Node.js environment, this is a great solution. We've created a smooth experience for you to run code on either Heroku or the Parse Cloud and we're very excited about the opportunities this combination has to offer.

To try it out, first link your Parse account with a Heroku account by visiting [your Parse account settings](https://parse.com/account/edit) and clicking the “Link Heroku” button.

Heroku support is integrated directly in the Parse command line tool, so you can use it just as you would for Cloud Code. “parse new” generates a small amount of initial code, and now gives you the option to use Node.js running on Heroku instead of the Cloud Code environment.

You can modify this code as you wish and deploy with “parse deploy.”

Both the [command line tool](https://parse.com/docs/cloudcode/guide#command-line-heroku) and the Node.js scaffolding are open source, so if you have ideas for how to improve it, check out the [Parse command line tool repo](https://github.com/ParsePlatform/parse-cli) and the [Node.js middleware repo](https://github.com/ParsePlatform/CloudCode-Express) on GitHub. We would love to hear any feedback or suggestions you have. Happy building!