---
id: 3949
title: What is Parse Server?
date: 2016-03-01T10:31:09+00:00
author: foscomarotto
layout: post
guid: http://blog.parse.com/?p=3949
permalink: /announcements/what-is-parse-server/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
hide_from_index:
  - "0"
image: /wp-content/uploads/2016/01/Open-DB-Launch.png
categories:
  - Announcements
tags:
  - open source
---
In this post, I will attempt to convince you that <a href="https://github.com/ParsePlatform/parse-server" target="_blank">Parse Server</a> is worthy of your time and interest, even if you've never heard of Parse before. I will also make the case that <a href="https://github.com/ParsePlatform/parse-server" target="_blank">Parse Server</a> is and can be objectively better than the hosted Parse service. I hope that by the end of it, you will consider evaluating it for your next project.

<u>**Mobile Developer Tools**</u>

Most modern applications store data and interact with other services on the internet. User accounts, shared content, documents and purchases; these things all need to be communicated and stored somewhere else. All of these applications we use require complementary server-side applications with which to communicate.

For a decade or so before Parse, everyone had to build this stuff individually. It required knowledge across many disciplines, including network and hardware maintenance, server-side development and scaling, and front-end development and design. Very few could do it all alone; the rest grouped up and worked with others to complete the whole picture.

The concept behind Parse has always been a simple one. Abstract away almost the entire process, allowing a solo mobile developer to build the next great mobile app. For several years now, Parse has been the easiest and most comprehensive platform for mobile-focused developers. This is why so many people were upset when Parse announced the hosted service would be retired in one year.

<u>**For Those Afraid Of Change**</u>

If what you demand is a hands-off hosted Parse service, that will still be available to you from other providers. They have already begun to pop up less than a month after the announcement, with at least <a href="https://nodechef.com/parse-server" target="_blank">one service</a> already operational. That said, I encourage you to investigate the benefits of Parse Server, even if it pushes you slightly outside of your comfort zone.

<u>**The Fastest Way To Build Mobile Apps**</u>

Since Parse Server is a self-hosted application, it does require you to take over some of the work Parse was doing. For most apps, this is not a lot of work or complexity. There are already very simple <a href="https://github.com/ParsePlatform/parse-server#getting-started" target="_blank">one-click deployments</a> and thorough guides available <a href="https://github.com/ParsePlatform/parse-server/wiki#community-links" target="_blank">online</a>. It takes about 5 minutes to get Parse Server running, either on your local machine or a cloud-based server. After just a few minutes, you're ready to start saving and querying data from your mobile app.

<u>**Why Parse Server is better, Head to Head**</u>

For the three years I've been at Parse, some of the major feature requests have stayed the same. Only now in open source can many of them be addressed. Some of the big differences can be seen here:

<center>
  <img src="{{ site.url }}/assets/wp-content/uploads/2016/02/comparison3.png" alt="Parse vs Parse Server" />
</center>

One of the biggest wins for Parse Server is that you can develop and test your application locally. You don't realize how awesome this is until you have it, when you don't have to deploy to the cloud after fixing a typo. The cycle time for testing approaches zero with Parse Server.

Parse was hosted on Amazon Web Services, in the east-coast data center, and offered no other hosting options. Parse Server can be hosted anywhere, and you even have the option of running multiple instances in different regions to serve a global audience.

For data storage, Parse has always used and built new functionality upon MongoDB. For file storage, Parse stored all files in their Amazon S3 bucket. With Parse Server, adapters are being written to allow developers to control which database platform and file storage system they'd like to use. It's early yet, but there are already two file adapters available and work is proceeding on multiple database adapters.

Parse offered a manual backup option, providing JSON files of your data. These backups could be imported back, but it's not the same as a true backup and restore feature which most databases provide. With Parse Server, in exchange for needing to bring your own database you get several benefits, including index management, performance tuning, backup and restore functionality, and all of the other features your database provides. Many small apps and prototypes may never need this advanced functionality, but it's nice to know that it's there if you do.

Parse enforced a 1,000-object maximum on queries, a 3-second time limit on database triggers, a 15-second time limit on cloud functions, and an overall 30-second limit on all requests. These were important limitations when running hundreds of thousands of apps, but are no longer necessary when you're only running your own. You're free to implement your own restrictions, but none are imposed upon you.

<u>**Parse Server Is Yours**</u>

<a href="https://github.com/ParsePlatform/parse-server" target="_blank">Parse Server</a>, and all of the client SDKs, are free and open source. More than that, we are actively engaging with everyone in the community to continue growing the Parse ecosystem. One month in, more than 40 contributors from around the world have committed code to Parse Server. The community has discussed features, reported bugs, and helped each other across almost 500 issues. More than 220 pull requests have come in, ranging from small fixes to large features, mostly from solo contributors but also large organizations like Microsoft and Amazon. People everywhere are jumping in to make Parse Server even better, easier to use, and easier to scale.

Whatever you're building, whatever problem you're solving, Parse can save you a whole lot of time getting started. We're looking forward to what happens next!