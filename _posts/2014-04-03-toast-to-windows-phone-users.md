---
id: 2000
title: A Toast to Windows Phone Users
date: 2014-04-03T09:00:55+00:00
author: andrewimm
layout: post
guid: http://blog.parse.com/?p=2000
permalink: /announcements/toast-to-windows-phone-users/
dsq_thread_id:
  - "3705317154"
categories:
  - Announcements
  - Engineering
---
Did you know that our Push Notifications product supports Windows devices? As developers, we know how important it is to maximize the ability to reach out to your users, regardless of which phone they might be using. For a rapidly growing subset of mobile phone owners, those devices are running Windows, and we wanted to make sure that sending notifications to those users was as easy and powerful as our existing offerings for iOS and Android.

With Windows Phone on our list of supported platforms, you can leverage our API to gain thorough control of when and where you send Windows Toast Notifications. Of course, if you've used us for Push before, you know that the simplest method for scheduling notifications has always been through our Push Console – don't worry, you'll find an option for Windows Phone there as well.

<p style="text-align: center;">
  <a href="{{ site.url }}/assets/wp-content/uploads/2014/01/Windows-Phone-Push-Console-e1389055831796.png"><img class=" wp-image-2008 aligncenter" alt="Windows Phone Push Console" src="{{ site.url }}/assets/wp-content/uploads/2014/01/Windows-Phone-Push-Console-e1389055831796.png" width="880" height="400" /></a>
</p>

You can configure your Windows Phone app to accept Push Notifications by following the instructions in our [Docs](https://parse.com/docs/push_guide#receiving/.NET). Once you have registered installations of your app, you'll be able to select the Windows Phone checkbox in the Push Console and see a preview of the Toast Notification that will appear on your users' devices. Selecting the times and recipients of your notifications is as easy as it always was with the Push Console – the query builder allows you to choose specific subsets of your users, making sure your message gets to exactly the right people.

As always, you can read about all of our Push Notification services in our [Docs](https://parse.com/docs/push_guide), including how to [track app opens through notifications](https://parse.com/docs/push_guide#receiving-tracking/.NET).