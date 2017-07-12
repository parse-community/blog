---
id: 2213
title: CyberAgent Uses Parse Push to Power Pashaoku Auction App
date: 2014-03-26T21:09:54+00:00
author: courtneywitmer
layout: post
guid: http://blog.parse.com/?p=2213
permalink: /non-technical/cyberagent-uses-parse-push-to-power-pashaoku-auction-app/
dsq_thread_id:
  - "3687615193"
categories:
  - Customers
  - Non-Technical
tags:
  - APAC
  - Japan
---
[<img style="border: 0pt none; float:right; padding-left:10px; padding-bottom:10px" alt="CyberAgent iTunes App Store screenshots" src="{{ site.url }}/assets/wp-content/uploads/2014/03/appstore.1.png" width="230" height="409" />]({{ site.url }}/assets/wp-content/uploads/2014/03/appstore.1.png)<a href="http://www.cyberagent.co.jp/en/" target="_blank">CyberAgent, Inc.</a> is a leading online media and advertising company based in Tokyo, Japan. In addition to their media ventures, the company also develops social games and other apps, including <a href="https://itunes.apple.com/us/app/ameba/id349442137?mt=8" target="_blank">Ameba</a>, a popular Japanese blogging platform. CyberAgent came to Parse when they developed <a href="http://www.pashaoku.jp/" target="_blank">Pashaoku</a>, a user-to-user auction service that became a subsidiary company, Pashaoku, Inc., in February of 2013.

Pashaoku (パシャオク) was designed with ease of use in mind, enabling users to create auction listings in less than 30 seconds. Optimized for smartphones, the app was developed based on the idea that all someone needs to post an auction is to snap a picture. In fact, the name says it all: "pasha" is the word for the "click" of a camera shutter, and "oku" is an abbreviation of the word for "auction."

In addition to allowing people to create their own instant auctions, the company collaborates with celebrities to auction personal items and opportunities to participate in special events with them. For example, the chance to go on a triple-date with Japanese comedy trio "Panther" was recently auctioned off for ¥567,001 (about $5,600 USD).

The team working on Pashaoku turned to Parse when they started looking for a service to send mass push notifications to users on both iOS and Android. <a href="https://parse.com/products/push" target="_blank">Parse’s Push</a> services became the main draw for CyberAgent, who use it to send out targeted notifications to users. For example, when someone creates a new auction, a notification is sent to all of his or her followers.

In addition to push, Pashaoku also stores targeting data in Parse. App developer Caesar Wirth, an iOS and Android developer who worked on the iOS version of Pashaoku, explains that, “Since Parse is so flexible, it is easy to add or remove targeting criteria. We keep Parse synchronized with our own database, so everything is always up to date.” After using Parse in Pashaoku, Caesar continues:

> I am a big fan of the data querying system and history log. We send pushes to certain subsets of users, and some of our queries can get a little complex. Parse handles it all. Afterwards, we can look back and see everything we sent, and to whom, so we can reproduce anything we want.
> 
> Parse provides such a stable and flexible infrastructure, that it would take many man hours to develop something internal. With the correct strategy, you could probably have something up and running within a matter of days as opposed to weeks. Even a free account provides many of the benefits.

With 520K+ downloads on <a href="https://itunes.apple.com/jp/app/pashaoku/id536462239?l=ja&ls=1&mt=8" target="_blank">iOS</a> and 300K+ on <a href="https://play.google.com/store/apps/details?id=jp.cyberagent.pashaoku" target="_blank">Android</a>, Pashaoku is still going strong long after its initial release. You can learn more about <a href="http://www.cyberagent.co.jp/en/" target="_blank">CyberAgent</a> and the story behind its success <a href="http://systemsofexchange.org/2014/03/cyberagent/" target="_blank">here</a>.