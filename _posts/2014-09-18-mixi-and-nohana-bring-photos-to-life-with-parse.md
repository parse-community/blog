---
id: 2470
title: Mixi and Nohana Bring Photos to Life With Parse
date: 2014-09-18T22:44:06+00:00
author: Nancy Xiao
layout: post
guid: http://blog.parse.com/?p=2470
permalink: /customers/mixi-and-nohana-bring-photos-to-life-with-parse/
post_format:
  - basic
dsq_thread_id:
  - "3685595902"
categories:
  - Customers
---
[<img class="alignnone wp-image-2471" style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2014/08/nohana.jpeg" alt="Nohana" width="400" height="225" />]({{ site.url }}/assets/wp-content/uploads/2014/08/nohana.jpeg)

As Japan's go-to destination for all things people and social, [Mixi](https://mixi.jp/) has its hands in a variety of creative outlets for bringing people together. With its roots as a social networking site, Mixi's recent foray into gaming with [Monster Strike](http://www.monster-strike.com/) has proven to be highly successful. Now, Mixi has developed a subsidiary named Nohana, a photobook creation app powered by Parse.

The Nohana app allows you to upload photos from your smartphone, assembling them into 20-page photo-booklets or albums that are then printed and sent straight to your door. The first book ordered in a month is free plus shipping, and users can invite family members to a secured group to share photos and make collaborative photobooks.

To date, over 100,000 photobooks have been published by over 200,000 people, uploading more than 4 million photos. For a time, Nohana was not only the number one free iOS app in Japan with over 1 million downloads, but it was also featured on national television.

With so many photos and memories flowing through the service each day, Nohana turned to Parse products for a stable and scalable way to develop a strong backend for its app. Photo data, purchase history, and user information are all stored in Parse. [Cloud Code](https://parse.com/tutorials/getting-started-with-cloud-code) is utilized for validation of a user's value input and for connecting with Twilio for SMS authentication.

For Kazuki Tanaka and Kenta Tsuji, developers on the Nohana team, the Parse experience has been key for the app's development and the startup's growth:

> The biggest merit of using Parse was shortening development of server-side infrastructure. To be a successful startup, we need to maximize output by minimizing cost and time, so it was perfect to use Parse.

Additionally, Nohana uses Parse Push to engage with its users, guiding them to photobook purchases and updates within the app. In the future, Nohana plans to use Parse Push to notify users of their shipment status. Of the features used, Parse has been especially useful for three elements of Nohana's development:

> 1. Administration of users are really useful—including being able to set access control for each set of data, authentication via email, password reset function;
> 
> 2. Implementation of server-side is easy;
> 
> 3. 3rd party integration is easy, too! (e.g. Twilio, Underscore.js)

The Nohana app is available on the [App Store](https://itunes.apple.com/jp/app/nohana-nohana-mei-yue1ce-wu/id593274820?mt=8) and the [Google Play Store](https://play.google.com/store/apps/details?id=jp.co.nohana&hl=en). Bring your memories to life in a flash!