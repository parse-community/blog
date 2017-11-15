---
id: 3386
title: MileIQ Finds a Mobile Backend that Goes the Distance with Parse
date: 2015-03-03T00:57:16+00:00
author: nancyxiao
layout: post
guid: http://blog.parseplatform.org/?p=3386
permalink: /customers/mileiq-finds-a-mobile-backend-that-goes-the-distance-with-parse/
post_format:
  - basic
app_store_link_id:
  - "578830929"
hide_from_index:
  - "0"
dsq_thread_id:
  - "3687725093"
categories:
  - Customers
tags:
  - core
  - finance
  - push
---
[<img class="alignnone wp-image-2704 size-medium" style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2015/03/mileiq-215x300.png" alt="MileIQ" width="215" height="300" />]({{ site.url }}/assets/wp-content/uploads/2015/03/mileiq.png)

"How much would you pay for an app that put more than $500 extra into your pocket each month without any effort or behavioral change on your part? That’s the question that MileIQ users are asking themselves after just a few days of using the mileage tracking and reporting app."  - <a href="http://pando.com/2015/01/29/mileiq-raises-11m-to-take-the-pain-out-of-mileage-tracking-and-put-more-cash-in-users-pockets/" target="_blank">PandoDaily, January 2015</a>

Since its launch in late 2013, <a href="https://www.mileiq.com/" target="_blank">MileIQ</a> has saved both time and money for countless users with its highly popular iPhone and Android apps. It has topped the charts as the #1 grossing Finance app in the App Store for many consecutive months, and continues to dominate headlines and home screens alike. Much of MileIQ's rousing popularity can be attributed to the app's acute focus on developing an incredibly delightful user experience for a very tedious user problem: mileage tracking and reimbursement.

Reimbursable miles are one of the most commonly audited things by both employers and the government.  However, an easy way for employees to log these miles is a problem less commonly tackled.  With MileIQ, employees can easily track the distance they've driven so they can efficiently and seamlessly file for a tax deduction or employer reimbursement.  This has proven to be hugely useful across the globe--and for the 50 million workers driving a significant amount for business in the U.S. alone.

The MileIQ app automates the capturing of miles, in addition to detecting the information the IRS needs--including departure, arrival, times, and distance.  With so much data to maintain and capture on-the-fly, the MileIQ team sought a robust mobile backend service.  From the team's early development stages, Parse became crucial for prototyping and eventually as a stable backend:

> <span class="first-sentence">The confidence we had in Parse was especially useful in the early stages of the development process--it enabled us to iterate rapidly towards a product that delights users with its ease of use across mobile and web.</span>

Everything from user registration to capturing geopoints are stored in Parse with the use of <a href="https://parse.com/products/core" target="_blank">Parse Core</a>.  In addition, the MileIQ team uses <a href="https://parse.com/products/push" target="_blank">Parse Push</a> to engage with drivers through highly targeted notifications, including an alert when a drive has been logged.  The team describes using Parse to be an influentially positive decision for their app:

> <span class="first-sentence">Parse enabled us to focus on each of the different parts of the system and reduce our development burden and operational overhead...</span> The Parse toolkit helped with everything from account passwords and provisioning to push notifications and in-app purchases--it is easy to recommend it as an indispensable tool for developing mobile first user experiences.

The MileIQ app is now available for download on the <a href="https://itunes.apple.com/us/app/id578830929?mt=8" target="_blank">App Store</a> and <a href="https://play.google.com/store/apps/details?id=com.mobiledatalabs.mileiq&referrer=af_tranid%3DC2JC4MC7ED6SRCT%26c%3Dandroid_homepage%26pid%3Dwebsite" target="_blank">Google Play Store</a>, and visit their website <a href="https://www.mileiq.com/" target="_blank">here</a>.