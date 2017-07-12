---
id: 2274
title: Take Your App Offline with Parse Local Datastore
date: 2014-04-30T17:30:18+00:00
author: grantlandchew
layout: post
guid: http://blog.parse.com/?p=2274
permalink: /announcements/take-your-app-offline-with-parse-local-datastore/
post_format:
  - basic
feature_image_style:
  - small
app_store_link_id:
  - ""
dsq_thread_id:
  - "3685570208"
hide_from_index:
  - "0"
categories:
  - Announcements
  - Engineering
---
How many times have you opened an app and stared at a loading screen for minutes, only to be confronted with the dreaded, "No internet connection"? Most mobile applications are simply clients that display data straight from a server, losing all functionality without an internet connection. How great would it be, though, if an app worked regardless of bad reception or lack of connectivity?

To make taking your app offline easier, we're excited to announce Parse Local Datastore.

Parse Local Datastore is a new feature in the Parse Android SDK that lets you to take your app offline with just a few simple lines of code. `ParseObject#pin()` adds `ParseObject`s to the Local Datastore and `ParseObject#unpin()` removes them. Once pinned, you can access them anytime with a normal `ParseQuery`:

<pre class="EnlighterJSRAW" data-enlighter-language="null">// Pin ParseQuery results
List&lt;ParseObject&gt; objects = query.find(); // Online ParseQuery results
ParseObject.pinAllInBackground(objects);

// Query the Local Datastore
ParseQuery&lt;ParseObject&gt; query = ParseQuery.get("Feed")
    .fromLocalDatastore()
    .whereEquals("starred", true)
    .findInBackground(new FindCallback() {
        public void done(List&lt;ParseObject&gt; objects, ParseException e) {
            // Update UI
        }
    });</pre>

Pinned `ParseObject`s will remain accessible while offline — through app exits and restarts — until they are unpinned. Subsequent saves, updates, or deletes will keep your Parse Local Datastore results up-to-date.

You can also choose to update your object using `ParseObject#saveEventually()`. This will first update its instance in the Local Datastore and then remotely, so any changes can be kept up-to-date while offline:

<pre class="EnlighterJSRAW" data-enlighter-language="null">ParseObject feed = ParseQuery.get(objectId); // Online ParseQuery result
feed.fetch();
feed.put("starred", true);

// No network connectivity
feed.saveEventually();

ParseQuery&lt;ParseObject&gt; query = ParseQuery.get("Feed")
  .fromLocalDatastore()
  .whereEquals("starred", true)
  .findInBackground(new FindCallback() {
    public void done(List&lt;ParseObject&gt; objects, ParseException e) {
      // "feed" ParseObject will be returned in the list of results
    }
  });</pre>

This also means that if you're already using `ParseObject#saveEventually()` to protect your app against network failures, you'll get most of this without any additional work, simply by updating to the latest Android SDK and enabling Local Datastore.

With Parse, it's easy to take your app offline! Check out our <a title="guide" href="https://parse.com/docs/android_guide#localdatastore" target="_blank">guide</a> and <a title="API docs" href="https://parse.com/docs/android/api/" target="_blank">API docs</a> to take your Android app offline now. Stay tuned for iOS, coming soon.