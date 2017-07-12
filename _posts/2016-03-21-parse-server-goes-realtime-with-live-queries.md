---
id: 3970
title: Parse Server Goes Realtime with Live Queries
date: 2016-03-21T11:35:05+00:00
author: mengyanwang
layout: post
guid: http://blog.parse.com/?p=3970
permalink: /announcements/parse-server-goes-realtime-with-live-queries/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Announcements
---
One of the key concepts behind Parse is the Query, which lets you tell the server which objects you need. The Query is based on a "pull" model. Every time you want to get new data, you run another query. For some applications, like realtime games or messaging systems, it's more convenient if you can get a constant stream of object updates.

To solve this problem, today we are introducing Parse Live Queries. To use live queries, you just construct a Query like you normally would and call `subscribe()` instead of `find()`. After that, whenever an object matching this query changes, the server will push the object data to you in realtime.

There is a catch - Live Queries only work with the open source Parse Server. They don't work with apps that are still using the [parse.com](https://www.parse.com/) hosted service. So this is yet another good reason to migrate to [Parse Server](https://github.com/ParsePlatform/parse-server) today. We strongly recommend you do this as soon as possible.

## Setting up Live Queries

There are many ways to set up Live Queries with Parse Server--in the same process, in different processes, or with separate servers. The simplest way is to set it up in the same process as Parse Server. Add one line to your Parse Server setup to tell it which classes should use live queries:

<pre class="line-numbers"><code class="language-javascript">
let api = new ParseServer({
  ...,
  liveQuery: {
    classNames: ['Player']
  }
});
</code></pre>

Then, it just takes a few lines of code to create the LiveQuery server:

<pre class="line-numbers"><code class="language-javascript">
// app is the Parse Server instance 
let httpServer = require('http').createServer(app);
httpServer.listen(port);
ParseServer.createLiveQueryServer(httpServer);
</code></pre>

Once your app gets larger, you might want a more complicated setup with more LiveQuery servers. You shouldn't have to worry about that for a while, but there are several ways to set up additional LiveQuery servers. You can see more details about setup [here](https://github.com/ParsePlatform/parse-server/wiki/Parse-LiveQuery#usage).

We've also provided a complete example for you [here](https://github.com/ParsePlatform/parse-server-example). 

## Using Live Queries

Today we are providing a LiveQuery client in our main [JavaScript SDK](https://github.com/ParsePlatform/Parse-SDK-JS) (v1.8) and a separate client package for [iOS](https://github.com/ParsePlatform/ParseLiveQuery-iOS-OSX). Let's use JavaScript for our example. In order to use a live query, you just `subscribe()` instead of calling `find()` on a regular Query.

<pre class="line-numbers"><code class="language-javascript">
let query = new Parse.Query('Player');
query.equalTo('name', 'Mengyan');
let subscription = query.subscribe();
</code></pre>

This `subscription` will receive updates for objects that match this query. For example, you can listen for the `create` event:

<pre class="line-numbers"><code class="language-javascript">
subscription.on('create', (person) => {
  console.log(person.get('favoriteAthlete'));
});
</code></pre>

After that, if someone creates a new `Player` object that matches this query, with data like:

<pre class="line-numbers"><code class="language-javascript">
{
  objectId: 'rj9f032j0k',
  name: 'Mengyan',
  favoriteAthlete: 'INnoVation',
  league: 'Platinum'
}
</code></pre>

Then the create event on the subscription will be triggered, and it will log `INnoVation` to the console.

There are other subscription events, like `update`, `delete`, `enter`, `leave`, and `error`. Check the [documentation](https://github.com/ParsePlatform/parse-server/wiki/Parse-LiveQuery-Protocol-Specification) for a more in-depth explanation of when to use each one.

All Live Queries functionality is open source, both client and server. You can check out the LiveQuery server code [here](https://github.com/ParsePlatform/parse-server), Javascript client code [here](https://github.com/ParsePlatform/Parse-SDK-JS) and iOS client code [here](https://github.com/ParsePlatform/ParseLiveQuery-iOS-OSX). For the complete doc, you can check LiveQuery server [here](https://github.com/ParsePlatform/parse-server/wiki/Parse-LiveQuery), Javascript client [here](https://www.parse.com/docs/js/guide#live-queries) and iOS client [here](https://github.com/ParsePlatform/ParseLiveQuery-iOS-OSX). We know we haven't built everything you might want in a realtime system. So if you have ideas for how to extend it, send in a pull request! Also, if you are still running an app on parse.com, don't forget that you can only use Live Queries after you've migrated to the open source [Parse Server](https://github.com/ParsePlatform/parse-server). Thanks for reading!