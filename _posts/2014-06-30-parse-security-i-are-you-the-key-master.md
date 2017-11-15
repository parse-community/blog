---
id: 2408
title: 'Parse Security I - Are you the Key Master?'
date: 2014-06-30T20:54:56+00:00
author: bryanklimt
layout: post
guid: http://blog.parseplatform.org/?p=2408
permalink: /learn/engineering/parse-security-i-are-you-the-key-master/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3679488601"
categories:
  - Engineering
tags:
  - security
  - tutorial
---
So, you've finished version 1 of your app, and you're ready to send it out into the world. Like a child leaving the nest, you are ready to push your app out to the various app stores and wait for the glowing reviews to come streaming in. Not so fast! You wouldn't send a child out into the world without teaching them how to protect themselves. Likewise, you shouldn't send your app out into a user's hands without taking some time to secure it using industry-standard best practices. After all, if your app gets compromised, it's not only you who suffers, but potentially the users of your app as well. In this five-part series, we'll take a look at what you can do to secure your Parse app.

[<img class="alignright size-full wp-image-2409" src="{{ site.url }}/assets/wp-content/uploads/2014/06/Edit_Your_App___Parse.png" alt="Parse App Keys" width="971" height="535" />]({{ site.url }}/assets/wp-content/uploads/2014/06/Edit_Your_App___Parse.png)

Security starts with understanding your app's keys. A Parse app has several different keys: a client key, a REST key, a .NET key, a JavaScript key, and a master key. All of the keys--other than the master key--have basically the same permissions. They are just intended for use on different platforms. So, we usually refer to any of those keys as a “client key." The first thing you need to understand is that **your client key is not a security mechanism**. It's not even intended to serve as such. Your client key is shipped as a part of your app. Anyone can decompile your app or proxy network traffic from their device and see your client key. With JavaScript, it's even easier. One can simply “view source” in the browser and immediately know your key. That's why Parse has many other security features to help you secure your data. The client key is given out to your users, so anything that can be done with just the client key is doable by the general public, even malicious hackers.

The master key, on the other hand, is definitely a security mechanism. Using the master key allows you to bypass all of your app's security mechanisms, such as class-level permissions and ACLs. Having the master key is like having root access to your app's servers. You should guard your master key with the same zeal with which you would guard your production machines' root password. Never check your master key into source control. Never include your master key in any binary or source code you ship to customers. And above all, never, ever give your master key out to strangers in online chat rooms. Stranger danger!

In Part II, we'll take a look at Parse's advanced features, which allow you to control what people with your client key can do.

<span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/07/parse-security-ii-class-hysteria/" target="_blank">Part II</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/14/parse-security-iii-are-you-on-the-list/" target="_blank">Part III</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/21/parse-security-iv-ahead-in-the-cloud/" target="_blank">Part IV</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/28/parse-security-v-how-to-make-friends/" target="_blank">Part V</a></span>