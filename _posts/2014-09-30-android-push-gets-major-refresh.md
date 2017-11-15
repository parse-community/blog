---
id: 2540
title: Android Push Gets Major Refresh
date: 2014-09-30T18:25:31+00:00
author: thomasbouldin
layout: post
guid: http://blog.parseplatform.org/?p=2540
permalink: /learn/android-push-gets-major-refresh/
post_format:
  - basic
dsq_thread_id:
  - "3609712960"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Learn
tags:
  - android
  - push
---
We're excited to announce today that the Android Push API is getting its biggest facelift since inception. We've rethought the API to bring Parse Push better in line with both other Parse APIs and traditional Android development. The new API simplifies developer setup, provides better reliability, and is much more easily extended or customized to override default push behavior.

With the new API, we've decoupled the concepts of registration and reaction; the icon and activity which you want push to use are no longer statically bound to a channel. You can replace all calls to

<pre class="EnlighterJSRAW" data-enlighter-language="js">PushService.subscribe(myApplicationContext, "channel", MyActivity.class, intIconID);</pre>

with

<pre class="EnlighterJSRAW" data-enlighter-language="js">ParsePush.subscribeInBackground("channel");</pre>

Notice the `InBackground` suffix in the new APIs on `ParsePush`? The Android API now exposes when the Parse servers have accepted a subscription change in addition to any reason why the request may have failed. When the new API receives a push, it fires the `com.parse.push.intent.RECEIVE` [Intent](http://developer.android.com/reference/android/content/Intent.html). We've provided a [BroadcastReceiver](http://developer.android.com/reference/android/content/BroadcastReceiver.html) implementation that automatically handles the vast majority of our developer's needs. You can register it with the following additions to your manifest:

<pre class="EnlighterJSRAW" data-enlighter-language="xml">&lt;receiver android:name="com.parse.ParsePushBroadcastReceiver"
  android:exported="false"&gt;
  &lt;intent-filter&gt;
    &lt;action android:name="com.parse.push.intent.RECEIVE" /&gt;
    &lt;action android:name="com.parse.push.intent.DELETE" /&gt;
    &lt;action android:name="com.parse.push.intent.OPEN" /&gt;
  &lt;/intent-filter&gt;
&lt;/receiver&gt;</pre>

This `BroadcastReceiver` will suit most developers needs without any additional configuration. Your pushes will show your application's default icon and launch your application's main activity when touched. To provide a different icon, specify it with the `com.parse.push.notification_icon` meta-data in your AndroidManifest.xml. You no longer need to add push open tracking calls throughout your activities; we do it automatically in the `BroadcastReceiver`. If the contents of your push are too long for the normal UI, we'll automatically create the expanded layout available in Android 4.1 JellyBean (API 16) and up.

We're also introducing a new special field for Android pushes too! If you specify the `uri` [push option](http://parse.com/docs/push_guide#options-data/Android), the Notification created will navigate to that URI rather than launching the default Activity. For example, the sending the following push from Cloud Code would create a notification that sends users to the Parse blog.

<pre class="EnlighterJSRAW" data-enlighter-language="js">Parse.Push.send({
  where: new Parse.Query(Parse.Installation), // Everyone
  data: {
    alert: "Android Push Gets Major Refresh",
    uri: "http://blog.parseplatform.org",
  }
})</pre>

When the user clicks 'back', they will be sent to your launcher Activity (overridden with `getActivity`). As a security precaution, the `uri` option may not be used when a push is sent from a client.

If your app has more sophisticated needs, you can easily subclass `ParsePushBroadcastReceiver` to tweak the behavior of your application without foregoing all of the built-in features. Subclass our `BroadcastReceiver` and put your new class in the manifest instead of ours. You can react to push notification events by overriding any of `onPushReceived`, `onPushOpen`, or `onPushDismiss`. If you need to customize the Notification that we create, you can override `getNotification`.

We're very excited for this new API. If you're not ready to adopt the new API quite yet, the old push APIs are deprecated but still work exactly as they did before. We hope you are excited for the new approach. It's now easier than ever to get started and to transition from turn-key implementations to much more sophisticated ones.