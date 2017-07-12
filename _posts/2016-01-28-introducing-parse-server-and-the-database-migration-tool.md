---
id: 3911
title: Introducing Parse Server and the Database Migration Tool
date: 2016-01-28T13:05:34+00:00
author: Fosco Marotto
layout: post
guid: http://blog.parse.com/?p=3911
permalink: /announcements/introducing-parse-server-and-the-database-migration-tool/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - large
image: /wp-content/uploads/2016/01/Open-DB-Launch.png
categories:
  - Announcements
---
Today, we are releasing two tools to help you transition your app from Parse to another service. With the <a href="http://blog.parse.com/announcements/moving-on/" target="_blank">announcement</a> that Parse's hosted service will be retired on January 28, 2017, we want to help share resources to make your migration as straightforward as possible.

### Parse Server, An Open-Source Parse API Server

The open source Parse Server makes it possible to serve the Parse API from any infrastructure that can host Node.js applications. Parse Server provides a way for you to keep your application running without major changes in the client-side code, once you have your data in your own database. Our client SDKs now support changing the API server location to direct them to your own. This also lets you use the Parse client SDKs with entirely new applications that have no dependency on the Parse hosted services.

Parse Server was built in Node.js and works with the Express web application framework. It can be added to existing web applications, or run by itself. Check out the server and migration guides <a href="https://parse.com/docs/server/guide#migrating" target="_blank">here</a>, the open-source repository <a href="https://github.com/ParsePlatform/parse-server" target="_blank">here</a>, and the example project <a href="https://github.com/ParsePlatform/parse-server-example" target="_blank">here</a>. We encourage you to provide bug reports and feedback via the GitHub issues on the Parse Server <a href="https://github.com/ParsePlatform/parse-server/issues" target="_blank">repository</a>. There's even a developer wiki, which can be found <a href="https://github.com/ParsePlatform/parse-server/wiki" target="_blank">here</a>.

Nearly everything you'll need for your app is supported in Parse Server, with the main exceptions of Analytics and Config.

### The Database Migration Tool

Starting today, you can now power your Parse app using your own MongoDB instance that you've hosted. Using your own MongoDB instance and infrastructure will help ensure that your queries will run at their highest level of performance. By migrating your database, you will be able to:

<ul class="standard-list">
  <li>
    backup your entire database periodically and on demand
  </li>
  <li>
    restore backups
  </li>
  <li>
    increase performance of queries (and thereby your app) by providing larger amounts of dedicated processing power and memory to your database
  </li>
  <li>
    eliminate risk of another app impacting the performance of your app's queries
  </li>
  <li>
    gain raw access to your application's data
  </li>
  <li>
    modify, add, or fine-tune indexes on your most popular or complex queries
  </li>
</ul>

We strongly encourage you to start the migration process as soon as possible. For more details, check out our migration guide <a href="https://github.com/ParsePlatform/parse-server/wiki/Migrating-an-Existing-Parse-App" target="_blank">here</a>.