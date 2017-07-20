---
layout: post
title: Important NodeJS Security Notice
date: 2017-07-19 17:20 -0700
comments: true
author: montymxb
categories: [Security, NodeJS, Update, Notice]
---
A [security issue](https://nodejs.org/en/blog/vulnerability/july-2017-security-releases/) was recently disclosed for Node.js. This affects versions of node starting at the 4.x line, all the way through 8.x.

As it stands security updates were released a week ago and are currently available for all active release lines (including 7.x). We _highly_ recommend you update your implementations to patch this issue (assuming you haven't already).

<!-- more -->

The issue in question is a type of high severity hash flooding attack. You can read more about the technical points of such an attack [here](https://events.ccc.de/congress/2011/Fahrplan/attachments/2007_28C3_Effective_DoS_on_web_application_platforms.pdf). The updated releases also include some general security patches. 

To give a general elaboration on this, the underlying issue had to do with releases of Node.js using a constant HashTable seed. Although the releases were intended to have randomized seeds it would appear building with V8 snapshots by default overwrote those randomized seeds. 

With the seed being constant, a malicious user could possibly perform a DoS attack exploiting this. Knowing the key in advance allows an individual to create intentional collisions, ultimately leading to a significant performance dip (in the best case). It's important to note this kind of attack is [nothing new](https://www.ruby-lang.org/en/news/2012/11/09/ruby19-hashdos-cve-2012-5371/), and is something be looked out for when handling untrusted user input that is destined for an entry in a hashtable.

As for staying up to date with future security issues (such as this) we recommend keeping up with the [nodejs security mailing list](https://groups.google.com/forum/#!forum/nodejs-sec). This is a fantastic way to catch wind of issues as soon as they're announced.
Staying on top of potential issues is a great way to keep your Parse Server running securely and safely.
