---
id: 3852
title: Open Sourcing Our Facebook and Twitter Integration Frameworks
date: 2015-10-08T13:11:50+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parse.com/?p=3852
permalink: /announcements/open-sourcing-our-facebook-and-twitter-integration-frameworks/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4207039087"
feature_image_style:
  - small
image: /wp-content/uploads/2015/10/ios-and-login.jpg
categories:
  - Announcements
tags:
  - android
  - authentication
  - iOS
  - users
---
In recent months, we've open sourced our iOS, Android, .NET and Javascript SDKs, marking a big step towards learning and building better than ever with the Parse community.

Today, we're excited to announce that we are open sourcing our Facebook and Twitter integration frameworks for iOS and Android. Both of these power one of the core features of Parse: **user authentication**. With these frameworks, logging in and linking your apps’ users with their Facebook or Twitter accounts is simple and seamless.

**Integrate with Facebook**

The Facebook integration is powered by ParseFacebookUtils on <a href="https://github.com/ParsePlatform/ParseFacebookUtils-iOS" target="_blank">iOS</a> and <a href="https://github.com/ParsePlatform/ParseFacebookUtils-Android" target="_blank">Android</a>. Due to deep integration with the latest Facebook SDK for each platform, you'll always stay ahead of the most up-to-date set of features and native experience.

It's easy to get started! Logging in your users through Facebook on iOS looks like this:

<pre class="line-numbers"><code class="language-objectivec">[PFFacebookUtils logInInBackgroundWithReadPermissions:@[ @"public_profile" ] block:^(PFUser *user, NSError *error) {
  if (!user) {
    NSLog(@"Uh oh. The user cancelled the Facebook login.");
  } else if (user.isNew) {
    NSLog(@"User signed up and logged in through Facebook!");
  } else {
    NSLog(@"User logged in through Facebook!");
  }
}];</code></pre>

Or on Android:

<pre class="line-numbers"><code class="language-java">ParseFacebookUtils.logInWithReadPermissionsInBackground(this, Collections.singleton("public_profile"), new LogInCallback() {
  @Override
  public void done(ParseUser user, ParseException err) {
    if (user == null) {
      Log.d("MyApp", "Uh oh. The user cancelled the Facebook login.");
    } else if (user.isNew()) {
      Log.d("MyApp", "User signed up and logged in through Facebook!");
    } else {
      Log.d("MyApp", "User logged in through Facebook!");
    }
  }
});</code></pre>

**Integrate with Twitter**

The Twitter integration is powered by ParseTwitterUtils on <a href="https://github.com/ParsePlatform/ParseTwitterUtils-iOS" target="_blank">iOS</a> and <a href="https://github.com/ParsePlatform/ParseTwitterUtils-Android" target="_blank">Android</a>. Here, we take a slightly different approach. On iOS, we initially try to look up a Twitter account in the system Accounts list. If no account is available, we present a user with a login dialog to login and grant access to your app. On Android, we always fallback to the login dialog.

Even though there is a lot of logic behind this, the experience will still be seamless on iOS:

<pre class="line-numbers"><code class="language-objectivec">[PFTwitterUtils logInWithBlock:^(PFUser *user, NSError *error) {
    if (!user) {
        NSLog(@"Uh oh. The user cancelled the Twitter login.");
        return;
    } else if (user.isNew) {
        NSLog(@"User signed up and logged in with Twitter!");
    } else {
        NSLog(@"User logged in with Twitter!");
    }
}];</code></pre>

As well as on Android:

<pre class="line-numbers"><code class="language-java">ParseTwitterUtils.logIn(this, new LogInCallback() {
    @Override
    public void done(ParseUser user, ParseException err) {
        if (user == null) {
            Log.d("MyApp", "Uh oh. The user cancelled the Twitter login.");
        } else if (user.isNew()) {
            Log.d("MyApp", "User signed up and logged in through Twitter!");
        } else {
            Log.d("MyApp", "User logged in through Twitter!");
        }
    }
});</code></pre>

These are just some of the open source elements that we've included to augment the experience of our SDKs. We are looking forward to your pull requests and contributions as well as seeing what you’ll build on Parse!