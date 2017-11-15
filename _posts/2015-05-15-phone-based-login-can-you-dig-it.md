---
id: 3538
title: Phone-Based Login, Can You Dig It?
date: 2015-05-15T10:16:00+00:00
author: foscomarotto
layout: post
guid: http://blog.parseplatform.org/?p=3538
permalink: /announcements/phone-based-login-can-you-dig-it/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3766845291"
feature_image_style:
  - large
image: /wp-content/uploads/2015/05/AnyPhone-hero.jpg
categories:
  - Announcements
  - Sample App
tags:
  - anyphone
  - cloudcode
  - twilio
---
What's quick, secure, and gives you instantly verified users?

Here at Parse, we love helping developers create the ultimate user experience, and that often includes building seamless, secure login and signup flows. For many apps, using a phone number as a login method has become the perfect combination of consistently fast and flawless, since people don't need to a create a username or choose a password to log in.

Using Parse, it's already possible to enable phone-based login in your app. But we wanted to make it even easier to get started for developers who think this type of login might benefit their app. So we created **AnyPhone, an open source iOS (Swift) and Cloud Code sample app that demonstrates phone number account creation**. With this sample app, you can learn how to build phone-based login with Parse using a simple, step-by-step example. Here's how it works:

When someone initially launches AnyPhone, or when they're logged out and open the app, AnyPhone asks for the user's phone number:

<div style="text-align: center;">
  <img class="aligncenter wp-image-3542" src="{{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.30.29-PM-576x1024.png" alt="anyphone-enter-phone" width="375" height="667" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.30.29-PM-576x1024.png 576w, {{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.30.29-PM-169x300.png 169w, {{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.30.29-PM.png 750w" sizes="(max-width: 375px) 100vw, 375px" />
</div>

## Text Message Verification

Once the person submits their phone number, they'll receive a text message at that number with a verification code. They enter that code on the next screen:

<div style="text-align: center;">
  <img class="aligncenter wp-image-3546" src="{{ site.url }}/assets/wp-content/uploads/2015/05/anyphone-2-576x1024.png" alt="anyphone-2" width="375" height="667" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/anyphone-2-576x1024.png 576w, {{ site.url }}/assets/wp-content/uploads/2015/05/anyphone-2-169x300.png 169w, {{ site.url }}/assets/wp-content/uploads/2015/05/anyphone-2.png 750w" sizes="(max-width: 375px) 100vw, 375px" />
</div>

## Reusable, Cross-Platform Login

That's it! Parse creates a User account for that person in the background on first login, which will be re-used on subsequent logins. Plus, it's cross-platform — the user can log in with their phone number and access the same account from multiple mobile devices and on the web. We've added some additional data fields that allow your app to update and save information about each user, such as their name, and maintain this information across multiple sessions or devices:

<div style="text-align: center;">
  <img class="aligncenter wp-image-3547" src="{{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.46.36-PM-576x1024.png" alt="anypic-main" width="375" height="667" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.46.36-PM-576x1024.png 576w, {{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.46.36-PM-169x300.png 169w, {{ site.url }}/assets/wp-content/uploads/2015/05/iOS-Simulator-Screen-Shot-May-14-2015-11.46.36-PM.png 750w" sizes="(max-width: 375px) 100vw, 375px" />
</div>

Check out the [GitHub repository](https://github.com/parseplatform/anyphone) here for setup instructions for the sample app. An account with [Twilio](http://twilio.com) is required.

With Parse and Cloud Code, there are so many capabilities available that the adventure can feel endless. We're excited to shine a light on more of these capabilities, and help you build better than ever before with sample apps like AnyPhone. We'd like to hear more about the clever things you've built on Parse—keep on sharing!