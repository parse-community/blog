---
id: 2386
title: Login Love for your Android App
date: 2014-06-25T18:47:54+00:00
author: stanleywang
layout: post
guid: http://blog.parse.com/?p=2386
permalink: /learn/engineering/login-love-for-your-android-app/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3678662565"
categories:
  - Engineering
tags:
  - android
  - login
---
We love UI components--as you may know through the pre-built UI classes in our iOS SDK. Today, we are bringing this same love to Android. We are launching <a title="Parse Login UI" href="https://www.parse.com/docs/android_guide#ui-login" target="_blank">ParseLoginUI</a>, an open-source library project for building login screens on Android with the Parse SDK. This ultra-customizable library implements screens for login, signup, and password help. We are releasing this as a standalone project (apart from the Parse Android SDK) so that you have the flexibility to make further changes to its look and feel when you integrate it into your app.

To use ParseLoginUI with your app, you should import the library project, and add the following to your app's `AndroidManifest.xml`:

<pre class="line-numbers"><code class="language-markup">&lt;activity 
    android:name="com.parse.ui.ParseLoginActivity" 
    android:label="@string/app_name" 
    android:launchMode="singleTop"&gt;
    &lt;meta-data 
        android:name="com.parse.ui.ParseLoginActivity.PARSE_LOGIN_ENABLED" 
        android:value="true"/&gt;
&lt;/activity&gt;</code></pre>

Then, you can show the login screen by launching `ParseLoginActivity` with these two lines of code:

<pre class="line-numbers"><code class="language-java">ParseLoginBuilder builder = new ParseLoginBuilder(MyActivity.this);
startActivityForResult(builder.build(), 0);</code></pre>

Within `ParseLoginActivity`, our library project will automatically manage the login workflow. Besides signing in, users can also sign up or ask for an email password reset. The default version of each screen (login, signup, and recover password) is shown below.

[<img class="aligncenter size-large wp-image-2389" src="{{ site.url }}/assets/wp-content/uploads/2014/06/basic_screens-1024x561.png" alt="Basic Login Screens" width="584" height="319" />]({{ site.url }}/assets/wp-content/uploads/2014/06/basic_screens.png)

Let's see how we can configure our login to look different.  Make the following changes to our `AndroidManifest.xml`:

<pre class="line-numbers"><code class="language-markup">&lt;activity 
    android:name="com.parse.ui.ParseLoginActivity" 
    android:label="@string/app_name" 
    android:launchMode="singleTop"&gt;
    &lt;meta-data 
        android:name="com.parse.ui.ParseLoginActivity.PARSE_LOGIN_ENABLED" 
        android:value="true"/&gt;

    &lt;!-- Added these options below to customize the login flow --&gt;
    &lt;meta-data 
        android:name="com.parse.ui.ParseLoginActivity.APP_LOGO"  
        android:resource="@drawable/app_logo"/&gt;
    &lt;meta-data  
        android:name="com.parse.ui.ParseLoginActivity.FACEBOOK_LOGIN_ENABLED"  
        android:value="true"/&gt;
    &lt;meta-data  
        android:name="com.parse.ui.ParseLoginActivity.TWITTER_LOGIN_ENABLED"  
        android:value="true"/&gt;
    &lt;meta-data  
        android:name="com.parse.ui.ParseLoginActivity.PARSE_LOGIN_HELP_TEXT"  
        android:value="@string/reset_password"/&gt;
    &lt;meta-data  
        android:name="com.parse.ui.ParseLoginActivity.PARSE_LOGIN_EMAIL_AS_USERNAME"  
        android:value="true"/&gt;
&lt;/activity&gt;</code></pre>

With these simple configurations, we've changed the app logo, added Facebook & Twitter logins, and changed the text shown for the password-reset link. We also enabled an option to automatically save the email address as the username, so that you don't have to manually save it in both fields of the `ParseUser` object. When this option is turned on, both the login and signup screens are also automatically updated to prompt for email address as username.

[<img class="aligncenter size-large wp-image-2390" src="{{ site.url }}/assets/wp-content/uploads/2014/06/customized_screens-1024x564.png" alt="Customized Login Screens" width="584" height="321" />]({{ site.url }}/assets/wp-content/uploads/2014/06/customized_screens.png)

Our Android <a title="Parse Login UI Documentation" href="https://www.parse.com/docs/android_guide#ui-login" target="_blank">documentation</a> contains guides for both basic and advanced use cases. You can find the source code for ParseLoginUI at our GitHub <a title="ParseUI repository" href="https://github.com/ParsePlatform/ParseUI-Android" target="_blank">repository</a>. Try it out and let us know what you think!