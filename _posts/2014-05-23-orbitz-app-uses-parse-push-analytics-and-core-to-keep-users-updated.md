---
id: 2337
title: Orbitz App Uses Parse Push, Analytics, and Core to Keep Users Updated
date: 2014-05-23T20:03:53+00:00
author: courtneywitmer
layout: post
guid: http://blog.parse.com/?p=2337
permalink: /non-technical/orbitz-app-uses-parse-push-analytics-and-core-to-keep-users-updated/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3678794636"
categories:
  - Customers
  - Non-Technical
tags:
  - analytics
  - push
  - travel
---
[<img style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2014/05/orbitz1.png" alt="Orbitz app on iPhone 5" width="300" height="515" />]({{ site.url }}/assets/wp-content/uploads/2014/05/orbitz1.png)The award-winning <a href="https://itunes.apple.com/us/app/orbitz-flights-hotels-cars/id403546234?mt=8" target="_blank">Orbitz Flights, Hotels, Cars app</a> for iOS gives iPhone and iPad users a slick, easy way to book and manage travel. With a fast shopping experience, streamlined booking process, contextual home screen, and details of booked trips, the Orbitz app brings information important for the user’s travel to their fingertips when they need it. <a href="https://parse.com/products/push" target="_blank">Parse Push</a>, <a href="https://parse.com/products/analytics" target="_blank">Analytics</a>, and <a href="https://parse.com/products/core" target="_blank">Core</a> help the app keep customers on top of their travel plans with up-to-the-minute notifications and by streamlining their search experiences.

Joining the mobile trend in 2006 with a mobile website that allowed customers to look up itineraries and check flight status while traveling, Orbitz launched the first version of the Orbitz iPhone app in 2010 once they realized that online booking via phone was becoming more and more prevalent. After launching on [iPhone](https://itunes.apple.com/us/app/orbitz-flights-hotels-cars/id403546234?mt=8) and [Android](https://play.google.com/store/apps/details?id=com.orbitz) in 2010, they transitioned their full-service iPhone app to be universal so that iPad users could book flights, hotels, and rental cars via a single app and added a [Kindle Fire edition](http://www.amazon.com/gp/mas/dl/android?p=com.orbitz), as well.

Principal Engineer on the iOS development team for Orbitz, Matt Sellars is focused on creating the best travel app experience possible for the company’s customers. With over 5 years experience at Orbitz, Matt came across Parse while chatting with other mobile developers about available platforms. “Simple APIs across many platforms with good documentation,” won him over, and now the app uses Parse to store a user’s search history and to push relevant travel alerts to them.

The app uses Parse Push to notify app users of changes in their trips, such as flight delays, gate changes, or cancellations, on the day that they travel. Matt and his team then use Parse Analytics to monitor usage of the push notifications they send.

The app also uses Parse Core to keep users’ recent Orbitz searches available and to remove old data. The app seamlessly synchronizes search information between any of a user’s devices so that he or she can easily pick up a previous search on a different device without having to re-enter the search criteria. The app also uses Cloud Code to remove expired data for users, allowing the app to move processing from a user’s device to the server and lowering the user’s data usage by removing irrelevant data before the apps fetches it.

According to Matt,

> Parse was a huge help for rapid feature development. It took one developer, myself, a couple days of reading and playing with the APIs to build and demo a feature. This normally would have required a few people to create the required backend to support building the prototype at a much higher cost. This enabled us to learn quickly, faster, and overall lower the cost to make an idea tangible for our stakeholders. This ability to rapidly prototype ideas allows our developers and designers to move quickly on ideas as if working in a startup environment together.

After all of the team’s investments in mobile over the last few years – both in apps and in optimizing their website for smartphone and tablet users – they now see nearly 1 in 3 hotel bookings being made via a mobile device. Positive reviews in the app stores make it clear that customers really enjoy using the apps, which have also been acknowledged by Apple as an Editors’ Choice and inducted into the App Store Hall of Fame in 2012, and by Google, who recently named Orbitz a “Top Developer” on Google Play for their Android app.

_The app is available for free download on [iOS](https://itunes.apple.com/us/app/orbitz-flights-hotels-cars/id403546234?mt=8), [Android](https://play.google.com/store/apps/details?id=com.orbitz), and [Kindle Fire](http://www.amazon.com/gp/mas/dl/android?p=com.orbitz) and is deeply integrated with the [Orbitz Rewards loyalty program](http://www.orbitz.com/rewards/). App users receive higher percentages of Orbucks, a loyalty currency that can be used on bookings, than they would by booking on the website.  _