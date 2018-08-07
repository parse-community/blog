---
id: 2334
title: Move Uses Parse Push to Power Notifications in Realtor.com App
date: 2014-05-16T18:30:09+00:00
author: courtneywitmer
layout: post
guid: http://blog.parseplatform.org/?p=2334
permalink: /customers/move-uses-parse-push-to-power-notifications-in-realtor-com-app/
dsq_thread_id:
  - "3692630924"
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Customers
tags:
  - iOS
  - Parse Push
  - push notifications
---
[<img style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2014/05/realtor.com_-554x1024.png" alt="Realtor.com App on iPhone 5" width="200" height="369" />]({{ site.url }}/assets/wp-content/uploads/2014/05/realtor.com_.png)The <a href="http://www.realtor.com/mobile" target="_blank">Realtor.com apps</a> by <a href="http://www.move.com/" target="_blank">Move, Inc.</a> provide a tailored search experience for real estate, helping people find the right home with features like photo-centric search, search by school, and map sketch-a-search. The only app to get real estate listings sourced directly from over 800 MLS’s with 90% of listings refreshed every 15 minutes, users of the app can also get updates on price reductions via push notification.

Real estate is all about data, and Move gets real-time data feeds from 98% of the multiple listing services (MLSs) in the United States. Managing that data is a big challenge, and it’s the duty of Alan Lewis, Principal Platform Architect for Move, to guide the development of the dozens of web services that make that data usable by the apps and sites Move supports.

When Alan, a 15 year veteran of the industry, started looking to improve push notification capabilities to support future growth for the company’s apps, he attended Parse Developer Day in San Francisco to learn about <a href="https://www.parse.com/products/push" target="_blank">Parse Push</a>. Although more research stood before him, he had an inkling Parse was the right choice when, within a few hours while still at the conference, he had a prototype using the service up and running.

The Move team implemented Parse Push to power notifications in their iOS apps. A key factor in this decision was the extra work inherent in supporting push in iOS, which requires that a developer build out additional server-side infrastructure or use a 3rd party services. According to Alan, the key to their decision to use Parse to fulfill this need were, “scale, cost, and developer-friendliness.”

He continues on to explain that,

> Parse is a great fit for apps that don’t have a server-side infrastructure, but for a large app like realtor.com where there are multiple existing services that supply the app’s data, what was important for me was to find a platform that could integrate seamlessly with ours. As an architect, I love what I see with Parse, and we can integrate with it via their web services without making compromises within our platform.
> 
> Developers should be informed about their options. Best-of-breed services like Parse can save a lot of time and money, and whether you’re a solo developer or working for a big company, there are probably far better uses of your time than building infrastructure that isn’t a key differentiator for your business. For us, push notifications were a must-have, but we weren’t going to gain any advantage by building out the feature from scratch, which is why we chose to go the 3rd-party route. I evaluated all of them, and Parse is the best.

_You can download the realtor.com app and the new Rentals app <a href="http://www.realtor.com/mobile" target="_blank">here</a>._