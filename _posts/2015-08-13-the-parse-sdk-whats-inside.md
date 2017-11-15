---
id: 3675
title: 'The Parse SDK: What’s inside?'
date: 2015-08-13T10:58:35+00:00
author: grantlandchew
layout: post
guid: http://blog.parseplatform.org/?p=3675
permalink: /learn/the-parse-sdk-whats-inside/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - large
dsq_thread_id:
  - "4029706792"
image: /wp-content/uploads/2015/08/Parse_Illustration-21.jpg
categories:
  - Engineering
  - Learn
tags:
  - opensource
  - sdk
---
The Parse SDK has been and continues to be an important part of mobile development on Parse. As Parse developers, you've already gotten to know the Parse SDK from its public API, but today we [open sourced our SDKs](http://blog.parseplatform.org/announcements/open-sourcing-our-sdks/) so you'll finally be able to take a peek at its inner workings.

In this post, **we'll unpack a few of the most challenging aspects** of building the Parse SDKs — structuring an asynchronous API, decoupling architecture, and achieving API consistency. Over the next few weeks, we'll publish a series of blog posts diving into even more under-the-hood features of our SDKs.

* * *

## Asynchronous API

Some of the important functionalities of the Parse SDK include communicating over the network, persisting data to disk, and returning data to the developer so that they can update their UI. All of this needs to happen asynchronously, in parallel and off the main thread. With this in mind, it should be no surprise to you that the most important part of our SDK is how we do asynchronous programming. Last year, we released `Tasks` as a part of [Bolts](http://blog.parseplatform.org/announcements/lets-bolt/), a composable promise-based library that simplifies parallelism and concurrency. We mentioned that we used it internally to solve some of our concurrency issues, but now you can finally see the extent of it.

Almost all of our internal APIs are Task-based. We utilize them to simplify serial execution of asynchronous operations such as persisting a dependency chain of `ParseObject`s to the server as well as parallel asynchronous operations such as persisting batches of unrelated `ParseObject`s. It's so powerful that we're even able to manage splicing the two together into a single asynchronous operation.

<pre class="line-numbers"><code class="language-java">/**
&nbsp;* Saves a collection of objects in serial batches of leaf nodes.
&nbsp;*/
public Task&lt;Void&gt; deepSaveAsync(List&lt;ParseObject&gt; objects) {
&nbsp;&nbsp;&nbsp;&nbsp;if (hasCycle(objects)) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return Task.forError(new RuntimeException("Unable to save a ParseObject with a relation to a cycle"));
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;Task&lt;Void&gt; task = Task.forResult(null);

&nbsp;&nbsp;&nbsp;&nbsp;List&lt;ParseObject&gt; remaining = new ArrayList&lt;&gt;(objects);
&nbsp;&nbsp;&nbsp;&nbsp;while (remaining.size() &gt; 0) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&lt;ParseObject&gt; batch = collectLeafNodes(objects);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;remaining.removeAll(batch);

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// Execute each batch operation serially, awaiting until
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// the previous has completed.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;task = task.onSuccessTask((t) -&gt; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return saveAllAsync(batch);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;});
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;return task;
}

/**
&nbsp;* Saves batches of objects in parallel.
&nbsp;*/
public Task&lt;Void&gt; saveAllAsync(List&lt;ParseObject&gt; objects) {
&nbsp;&nbsp;&nbsp;&nbsp;if (objects.size() &gt; MAX_BATCH_SIZE) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// The collection of objects is too big for a single batch,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// so partition the collection and execute each batch
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// in parallel.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&lt;List&lt;ParseObject&gt;&gt; partitioned = Lists.partition(objects, MAX_BATCH_SIZE);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&lt;Task&lt;Void&gt;&gt; tasks = new ArrayList&lt;&gt;();
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for (List&lt;ParseObject&gt; partition : partitioned) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tasks.add(saveAllAsync(partition);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return Task.whenAll(tasks);
&nbsp;&nbsp;&nbsp;&nbsp;}

&nbsp;&nbsp;&nbsp;&nbsp;return executeBatchCommand(objects);
}

// * Abridged, for clarity</code></pre>

Writing asynchronous APIs is a breeze with `Tasks` and we highly recommend taking a deeper look both at Bolts Framework: [Android](https://github.com/BoltsFramework/Bolts-Android), [iOS/OS X](https://github.com/BoltsFramework/Bolts-iOS) and our SDKs: [Android](https://github.com/ParsePlatform/Parse-SDK-Android), [iOS/OS X](https://github.com/ParsePlatform/Parse-SDK-iOS-OSX)

## Decoupled Architecture and API Consistency

Keeping the Parse SDK experience as easy and delightful as possible is one of our highest priorities. However, it's hard to add new features and make our SDK more robust without making breaking changes. Additionally, as our codebase grows, we need to ensure that our code stays testable so that we can deliver the most stable experience to our developers.

To solve all this, we've adopted a decoupled architecture model that consists of our public API object instances, object states, controllers, and REST protocol. Each piece is encapsulated to ensure separation of concerns and different implementations allow us to add new features without modifying too much code. Here's a diagram of how this all comes together:<figure id="attachment_3695" style="width: 1400px" class="wp-caption alignnone">

<img class="wp-image-3695 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/08/new-sdk-diagram.jpg" alt="" width="1400" height="800" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/new-sdk-diagram.jpg 1400w, {{ site.url }}/assets/wp-content/uploads/2015/08/new-sdk-diagram-300x171.jpg 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/new-sdk-diagram-1024x585.jpg 1024w, {{ site.url }}/assets/wp-content/uploads/2015/08/new-sdk-diagram-875x500.jpg 875w" sizes="(max-width: 1400px) 100vw, 1400px" /><figcaption class="wp-caption-text">Our decoupled architecture model.</figcaption></figure> 

## Object Instance

The object instance is the piece that allows us to maintain an easy-to-use and unchanging API. For `ParseObject`, this is the API surface layer that contains getting and setting `ParseObject` properties, as well as saving, fetching, and deleting. As long as we keep this intact, we can refactor and add new features on the underlying levels without any breaking changes.

## State

The state refers to the combination of the internal state of the object. For `ParseObject`, it is the current representation of itself on the server, the collection of mutations that have performed locally, and a cache of its current representation locally.

These state instances also define the interface in which the object instance and controller interact. The object instance passes its current state to the controller, the controller returns a new state back, and the object instance then updates itself with the new state.

## Controller

The controller defines all the actions that can be performed on each Parse type: A `ParseObject` can be saved, fetched, and deleted, a `ParseQuery` can be found and counted, and a `ParseFile` can be saved and fetched.

Our basic controllers serialize and deserialize our object states into our public REST format and pass along all requests to our internal networking logic. This prevents us from needing to complicate our instance and state implementations with unnecessary serialization and deserialization logic as well as affording us to be able to instrument our code for better testability.

We've also designed our controllers to be extendable to offer additional functionality other than communicating with Parse. One instance of this is Local Datastore. For this new feature, we were able to create another implementation of our `ParseQueryController`, but instead of communicating over the network with Parse it queried for objects locally on the device.

* * *

### **Stay Tuned for More**

In this post, we've covered a few major aspects of the Parse SDK architecture and how they were built. Stay tuned for future blog posts on topics like how we approach thread safety, our test philosophy, and more!
  
&nbsp;
  
**Ready to dig into the code?**
  
You can find it here for [Android](https://github.com/ParsePlatform/Parse-SDK-Android) and [iOS/OS X](https://github.com/ParsePlatform/Parse-SDK-iOS-OSX).