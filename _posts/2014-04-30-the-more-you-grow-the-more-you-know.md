---
id: 2284
title: The More You Grow, the More You Know
date: 2014-04-30T10:35:32+00:00
author: christineyen
layout: post
guid: http://blog.parseplatform.org/?p=2284
permalink: /announcements/the-more-you-grow-the-more-you-know/
post_format:
  - feat-image
app_store_link_id:
  - ""
feature_image_style:
  - small
dsq_thread_id:
  - "3688557872"
image: /wp-content/uploads/2014/04/Blog-post-Growth-Analytics.png
categories:
  - Announcements
  - Engineering
---
Understanding how your app behaves in the wild should be as seamless as possible. Parse Analytics has thus far focused on ease of integration and ease of use. For apps already part of the Parse ecosystem, this is even more true: we can provide value while removing the need to instrument every operation in your app. And now, with Facebook in our corner, we're able to leverage even greater resources to help you learn about your app's performance.

Today, we're pleased to announce the next steps in our journey: the new "Audience" and "Retention" tabs in your Analytics dashboard. After you've built your mobile app, the next step is to build, grow, and retain your user base. Loyal users are key to an app's long-term success, and with these new growth-centric insights, you'll be able to keep track of your users better than ever before.

**How many people are using my app? And how often?**

Under the new Audience tab, we expose information about your Daily, Weekly, and Monthly active [Users](https://parse.com/docs/ios_guide#users/iOS) and [Installations](https://parse.com/docs/push_guide#installations/iOS). While these links are to the iOS version of these docs, Users and Installations are available across all platforms. You’re likely using one or both in your app already!

With these newest additions to your Analytics dashboard, you can see growth week over week per stat, as well as daily over time.

[<img class="aligncenter size-full wp-image-2287" src="{{ site.url }}/assets/wp-content/uploads/2014/04/growthanalytics-audience.png" alt="growthanalytics-audience" width="1015" height="852" />]({{ site.url }}/assets/wp-content/uploads/2014/04/growthanalytics-audience.png)

As always, we’ve got your data in multiple formats: compare Daily Active Users to Monthly Active Users, take a look at week-over-week changes to your Daily Active stats, or watch your Weekly Actives trend over time.

Curious how to read these numbers? We’ve brought in our expert analyst, Dev, to chat about how to interpret these charts and what sorts of patterns to keep an eye out for:



**Now that I’ve got users, how do I make sure they’re sticking around?**

You’ve been able to [track your app activity](http://blog.parseplatform.org/2013/07/17/api-request-analytics-better-faster-more-insightful/) for months, so the natural next step for us was to combine activity data with something more meaningful to you: your users. You can now track retention of your user base — in other words, how many of your users continue to return to your app day after day.

[<img class="aligncenter size-full wp-image-2307" src="{{ site.url }}/assets/wp-content/uploads/2014/06/growthanalytics-retention.png" alt="The Retention tab of our new Growth Analytics." width="1015" height="645" />]({{ site.url }}/assets/wp-content/uploads/2014/06/growthanalytics-retention.png)

The rows of this table are divided into user cohorts by signup date. As you travel from left to right along a single row, you travel forward in time in that particular cohort’s lifetime: the further right you go, the longer it’s been since the user signed up, and the stickier your app.

The columns of the table, then, each represent “day X since the user signed up.” As you travel down the leftmost column, you’re examining the rate at which users returned 1 day after signup, across cohorts.

While some fluctuation is normal, this Retention tab will provide key insight into when users stop coming back, or which of your features affect your product’s stickiness the most. Take some time to play around with the interface and see what you can learn about your app! Here's another screencast from Dev with some sample data and handy tips on how to work with this chart:



**Coming to an inbox near you: Daily Parse stats for everyone!**

At Parse, we’ve been tracking these sorts of metrics ourselves as well: we rely on a daily email, called the Daily Parse, to keep an eye on our audience and retention metrics over time. As a final part of our Growth Analytics offering, we’re bringing this “analytics-in-your-inbox” to all of you.

Starting tomorrow, developers will receive daily updates on how their apps have performed over the last day. There will be information about user signups, installation activations, API requests, and push notifications. You can customize the emails to only include data from certain apps, or opt out of these emails entirely, via your [account settings](https://parse.com/account/notifications). We hope you’ll find them as useful as we do!

[<img class="aligncenter size-full wp-image-2313" src="{{ site.url }}/assets/wp-content/uploads/2014/06/growthanalytics-dailyparse.png" alt="A sample Daily Parse email" width="1015" height="599" />]({{ site.url }}/assets/wp-content/uploads/2014/06/growthanalytics-dailyparse.png)

Parse is committed to providing robust analytics that emphasize our core strengths: clarity, consistency, and simplicity. We hope you like the new angles we’ve provided into how your app is doing, and as always, let us know what you think! We’ve got more good things to come.