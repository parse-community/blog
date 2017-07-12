---
id: 2016
title: Parse Push Adds Support for GCM
date: 2014-01-14T18:34:39+00:00
author: bennham
layout: post
guid: http://blog.parse.com/?p=2016
permalink: /learn/engineering/parse-push-adds-support-for-gcm/
dsq_thread_id:
  - "3687529277"
categories:
  - Engineering
---
The Parse Android SDK now supports using Google Cloud Messaging (GCM) to receive push notifications. This allows your Parse app to receive pushes over a connection that is shared with other apps that use GCM, which maximizes battery life.

To add GCM support to an existing Parse app, [download](https://parse.com/docs/downloads) the latest Android SDK, and add the following elements to your app's manifest as children of the root `<manifest>` element:

<pre class="brush: markup; gutter: false">&lt;uses-permission android:name="android.permission.WAKE_LOCK" /&gt;
&lt;uses-permission android:name="android.permission.GET_ACCOUNTS" /&gt;
&lt;uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" /&gt;

&lt;!--
  IMPORTANT: Change "com.example.app.permission.C2D_MESSAGE" in the lines below
  to match your app&#039;s package name + ".permission.C2D_MESSAGE".
--&gt;
&lt;permission android:protectionLevel="signature"
    android:name="com.example.app.permission.C2D_MESSAGE" /&gt;
&lt;uses-permission android:name="com.example.app.permission.C2D_MESSAGE" /&gt;</pre>

Also, add the following elements to your app's manifest as children of the `<application>` element:

<pre class="brush: markup; gutter: false">&lt;receiver android:name="com.parse.GcmBroadcastReceiver"
    android:permission="com.google.android.c2dm.permission.SEND"&gt;
  &lt;intent-filter&gt;
    &lt;action android:name="com.google.android.c2dm.intent.RECEIVE" /&gt;
    &lt;action android:name="com.google.android.c2dm.intent.REGISTRATION" /&gt;

    &lt;!--
      IMPORTANT: Change "com.example.app" to match your app&#039;s package name.
    --&gt;
    &lt;category android:name="com.example.app" /&gt;
  &lt;/intent-filter&gt;
&lt;/receiver&gt;</pre>

Your Parse app will now use GCM to receive pushes without any changes to existing code. On devices like the Kindle Fire where GCM is not supported, Parse will continue to use a background service to receive pushes from the Parse Cloud, so you can upgrade your app without fear of breaking existing apps.

If you're interested in adding push support to a new application, get started with our [Android Push Tutorial](https://parse.com/tutorials/android-push-notifications), and check out the [Android Push Guide](https://parse.com/docs/push_guide#top/Android) for more details.