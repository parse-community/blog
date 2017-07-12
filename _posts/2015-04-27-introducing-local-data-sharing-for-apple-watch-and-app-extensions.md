---
id: 3486
title: Introducing Local Data Sharing for Apple Watch and App Extensions
date: 2015-04-27T13:01:29+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parse.com/?p=3486
permalink: /announcements/introducing-local-data-sharing-for-apple-watch-and-app-extensions/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - small
dsq_thread_id:
  - "3717667138"
image: /wp-content/uploads/2015/04/watch-post1.jpg
categories:
  - Announcements
tags:
  - appextensions
  - applewatch
  - WatchKit
---
Here at Parse, we're always focused on building tools that help you create powerful and successful apps. Last year, the introduction of iOS 8 and OS X Yosemite brought the ability for developers to extend their apps' functionality to more surfaces with App Extensions. With the launch of the Apple Watch and the ability to use App Extensions with WatchKit, developers can now extend their iOS apps onto Apple Watch. We're excited by the possibilities of this new wearable platform, and we wanted to make it easier for Parse apps to work with the Apple Watch.

Today, we're proud to announce local data sharing support for Apple Watch and App Extensions in our [iOS](https://parse.com/products/ios) and [OS X](https://parse.com/products/osx) SDKs. This means that your app's local data will always be seamlessly available and in sync between your app and its extensions as well as Apple Watch apps.

Previously, if you built an extension, in order to authenticate a user you'd typically need to prompt them to log in more than once (both on their phone and on their Apple Watch). Or, you'd need to find an alternative way to share the user session token and help a user login manually. If you had a list of objects and wanted to make it available locally to your extension, there was no way to accomplish this aside from refetching these objects in your app extensions and persisting them locally in two places.

Now, with our latest SDKs, we've greatly simplified this process. Upon adding a data sharing flag to your app, all data that you persist in the local datastore, including the current authenticated user and the current installation, are all available in your App Extensions and Apple Watch apps.

## How does it work?

Here's how you enable this functionality in your apps:

Enable **App Groups** and **Keychain Sharing** in <span style="text-decoration: underline">both</span> your app and extension capabilities in Xcode.<figure id="attachment_3489" style="width: 1103px" class="wp-caption alignnone">

<img class="wp-image-3489 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/04/LocalDataSharing_Extensions.png" alt="LocalDataSharing_Extensions" width="1103" height="582" srcset="{{ site.url }}/assets/wp-content/uploads/2015/04/LocalDataSharing_Extensions.png 1103w, {{ site.url }}/assets/wp-content/uploads/2015/04/LocalDataSharing_Extensions-300x158.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/04/LocalDataSharing_Extensions-1024x540.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/04/LocalDataSharing_Extensions-875x462.png 875w" sizes="(max-width: 1103px) 100vw, 1103px" /><figcaption class="wp-caption-text">Be sure to use the same identifiers for both the app and the extension!</figcaption></figure> 

Add the following before you initialize Parse in your main app:

### Objective-C:

<pre class="line-numbers"><code class="language-objectivec">// Enable data sharing in main app.
[Parse enableDataSharingWithApplicationGroupIdentifier:@"group.com.parse.parseuidemo"];
// Setup Parse
[Parse setApplicationId:@"&lt;ParseAppId&gt;" clientKey:@"&lt;ClientKey&gt;"];</code></pre>

### Swift:

<pre class="line-numbers"><code class="language-swift">// Enable data sharing in main app.
Parse.enableDataSharingWithApplicationGroupIdentifier("group.com.parse.parseuidemo");
// Setup Parse
Parse.setApplicationId("&lt;ParseAppId&gt;", clientKey: "&lt;ClientKey&gt;")</code></pre>

Add the following before you initialize Parse in your App Extension:

### Objective-C:

<pre class="line-numbers"><code class="language-objectivec">// Enable data sharing in app extensions.
[Parse enableDataSharingWithApplicationGroupIdentifier:@"group.com.parse.parseuidemo"
containingApplication:@"com.parse.parseuidemo"];
// Setup Parse
[Parse setApplicationId:@"&lt;ParseAppId&gt;" clientKey:@"&lt;ClientKey&gt;"];</code></pre>

### Swift:

<pre class="line-numbers"><code class="language-swift">// Enable data sharing in app extensions.
Parse.enableDataSharingWithApplicationGroupIdentifier("group.com.parse.parseuidemo",
containingApplication: "com.parse.parseuidemo");
// Setup Parse
Parse.setApplicationId("&lt;ParseAppId&gt;", clientKey: "&lt;ClientKey&gt;");</code></pre>

As you might have noticed, there are few pieces of information that need to be in sync for this to be enabled and function properly:

<ul class="standard-list">
  <li>
    <b>Application Group Identifier </b>is the identifier of a shared sandbox that the Parse SDK will use to persist data locally and share the data.
  </li>
  <li>
    <b>Keychain Sharing</b> means that your app and extensions can access data that is securely stored in the keychain.
  </li>
  <li>
    <b>Containing Application Identifier</b> is your main apps bundle identifier. This is required for app extensions to know which application to talk to.
  </li>
</ul>

This process is required for both your main application and your WatchKit app. It also works for any extension aside from WatchKit — after all these steps are completed, all data will be shared between your app and any app extension that the app contains.

Apple Watch apps and App Extensions add a whole world of possibilities to your apps, and Parse is here to make sure you have all the tools to focus on what makes your app awesome.

The latest SDKs are already available for <a title="Parse iOS/OSX SDKs" href="https://parse.com/docs/downloads" target="_blank">download here</a>.
  
We're excited to see what's coming to your apps.
  
<a href="https://groups.google.com/group/parse-developers" target="_blank">Let us know what you think!</a>