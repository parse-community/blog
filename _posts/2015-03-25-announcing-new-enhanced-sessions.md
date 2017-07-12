---
id: 3388
title: Announcing New Enhanced Sessions
date: 2015-03-25T10:50:03+00:00
author: stanleywang
layout: post
guid: http://blog.parse.com/?p=3388
permalink: /announcements/announcing-new-enhanced-sessions/
post_format:
  - feat-image
feature_image_style:
  - small
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3687559583"
image: /wp-content/uploads/2015/03/sessions-blog.jpg
categories:
  - Announcements
  - Engineering
tags:
  - security
  - sessions
---
At Parse, we're constantly striving to provide the tools you need to make your app more successful. One of the most important aspects of an app's success is security, so that people can trust your app when they use it. Parse already provides several tools to secure your app, including [Access Control Lists (ACLs)](https://parse.com/docs/data#security-objects) for Parse Objects, [Class-Level Permissions](http://blog.parse.com/2015/02/23/secure-your-app-one-class-at-a-time) for each data table on Parse, and [Cloud Code](http://blog.parse.com/2014/07/21/parse-security-iv-ahead-in-the-cloud/) for even more in-depth security customizations. Today, we're excited to announce an update to our previous sessions functionality. With the new Enhanced Sessions feature, we're giving developers and people more control than ever over data security.

Prior to this launch, if a user logged into your app through multiple devices, this same session token was shared across all devices. This session token was not revocable, and upon logging out, the token was not destroyed.

Now, with Enhanced Sessions, the Parse Cloud will assign a unique revocable `Session` object for each user on a given device for your specific app. This means different devices and web browsers will have different session tokens for the same user in your app. On mobile and web apps, you don't have to worry about creating and destroying sessions yourself. When users log in or sign up, the Parse Cloud automatically creates the corresponding `Session` object. When users log out, the Parse Cloud automatically destroys the corresponding `Session` object, which invalidates the session token that was previously assigned to that user's device.

We're also introducing a flexible Revocable Sessions API that lets you easily understand and securely manipulate user sessions in your app. You can see your app's `Session` objects in your Parse account's Data Browser, just like any other Parse object. If a user contacts you about his or her account being compromised in your app, you can use the Data Browser, REST API, or Cloud Code to forcefully revoke user sessions using the Master Key. These new APIs also allow you build a “session manager” UI screen where your app's users can see a list of all devices they've logged in with, and optionally log out of other devices. This empowers people who use your app to help monitor and protect their accounts. In addition, for increased security, you can configure your app to automatically expire user sessions due to inactivity.

<div style="text-align: center;">
  <a href="{{ site.url }}/assets/wp-content/uploads/2015/03/Screen-Shot-2015-03-24-at-4.26.53-PM.png"><img class="alignnone size-large wp-image-2725" src="{{ site.url }}/assets/wp-content/uploads/2015/03/Screen-Shot-2015-03-24-at-4.26.53-PM-1024x593.png" alt="Sessions" width="584" height="338" /></a>
</div>

Today, we also announced Parse for IoT. The Revocable Sessions API lets you implement scenarios where people can use your app on their phone to provision restricted user sessions for their IoT connected devices. This not only allows IoT devices to securely access user data, but also protects your users' accounts in case their IoT device gets stolen. For example, the session token on an IoT device cannot be used to modify `User` objects on Parse.

The new Revocable Sessions API is available on all Parse SDKs (iOS, Android, C#, JavaScript, PHP) as well as our REST API. All new Parse apps created after today will have Revocable Sessions enabled by default (configured in your app's Parse.com dashboard settings page). You can use existing login/signup/logout APIs in the same way as you did prior to this new feature. The only significant change is that you'll have to handle a new invalid session error on Parse API requests, and prompt the user to log in again when necessary. Your app will encounter this invalid session error when that device's user session token is revoked from elsewhere. This happens when your user logs out of that phone from the “session manager” screen of another phone, or when you (the app developer) use the Master Key to programmatically delete that `Session` object in the Parse Cloud.

If you currently have a production app on Parse, we recommend that you upgrade to the new Revocable Sessions API as soon as possible to take advantage of the new, more secure model with unique tokens for each device, and the ability to easily revoke user session tokens when necessary. We've prepared a [Session Migration Tutorial](https://parse.com/tutorials/session-migration-tutorial) to help you upgrade your app smoothly.

For more about our Revocable Sessions API, please see our [documentation](https://www.parse.com/docs/ios_guide#sessions/iOS).