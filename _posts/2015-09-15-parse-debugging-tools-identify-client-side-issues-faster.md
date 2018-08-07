---
id: 3801
title: 'Parse Debugging Tools: Identify Client-Side Issues Faster'
date: 2015-09-15T12:51:51+00:00
author: mengyanwang
layout: post
guid: http://blog.parseplatform.org/?p=3801
permalink: /announcements/parse-debugging-tools-identify-client-side-issues-faster/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4132865162"
categories:
  - Announcements
tags:
  - debug
  - loginterceptor
  - stethointerceptor
---
In order to build amazing apps you sometimes need to quickly debug issues. We recently launched the [Parse Explorer](http://blog.parseplatform.org/learn/chart-new-territory-with-parse-explorer/) tool to help with this process; it allows you to better investigate issues with your app’s API requests. However, even before diving into your API requests, you need to identify whether issues may be occurring on the client side instead. Today we're happy to introduce new Parse debugging tools for iOS and Android, which help you to identify client-side issues by surfacing requests and responses between the Parse SDK and Parse server.

* * *

## Debugging tools for Android

For Android, we've created two tools: the log interceptor and the Stetho interceptor. Let's take a look at each one.

The log interceptor will log all requests and responses between the Parse SDK and server to Android logcat. It's easy to integrate with your app. After you add `parseinterceptors-android` dependency, all you need to do is add one line code before you initialize Parse in your `Application#onCreate()`

<pre class="line-numbers"><code class="language-java">Parse.&lt;i>addParseNetworkInterceptor&lt;/i>(&lt;b>new &lt;/b>ParseLogInterceptor());
// Interceptor should be added before your initialize Parse
Parse.initialize(this);
....</code></pre>

Then, you can check the network information in Android logcat.

<img class="aligncenter size-large wp-image-3807" src="{{ site.url }}/assets/wp-content/uploads/2015/09/ParseLogInterceptor_ScreenShot-1024x524.jpg" alt="ParseLogInterceptor_ScreenShot" width="640" height="328" srcset="{{ site.url }}/assets/wp-content/uploads/2015/09/ParseLogInterceptor_ScreenShot-1024x524.jpg 1024w, {{ site.url }}/assets/wp-content/uploads/2015/09/ParseLogInterceptor_ScreenShot-300x154.jpg 300w, {{ site.url }}/assets/wp-content/uploads/2015/09/ParseLogInterceptor_ScreenShot-875x448.jpg 875w" sizes="(max-width: 640px) 100vw, 640px" />

The second tool is the Parse Stetho interceptor. [Stetho](https://github.com/facebook/stetho) is an open source library which allows you to debug your app through the Chrome Developer tool. With the help of the Parse Stetho interceptor, you can easily integrate Parse and Stetho: After you add Stetho and `parseinterceptors-android` dependency, you just need to add a few lines of code before you initialize Parse in your `Application#onCreate()`

<pre class="line-numbers"><code class="language-java">
// The stetho initialization may change if you use different Stetho version
Stetho.initializeWithDefaults(this);

Parse.addParseNetworkInterceptor(new ParseStethoInterceptor());
// Interceptor should be added before your initialize Parse
Parse.initialize(this);
....</code></pre>

Once you complete the set-up, start your app, point your laptop browser to `chrome://inspect` and click the "Inspect" button to begin.

<img class="aligncenter size-large wp-image-3806" src="{{ site.url }}/assets/wp-content/uploads/2015/09/image-1024x585.png" alt="image" width="640" height="366" srcset="{{ site.url }}/assets/wp-content/uploads/2015/09/image-1024x585.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/09/image-300x171.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/09/image-875x500.png 875w, {{ site.url }}/assets/wp-content/uploads/2015/09/image.png 1440w" sizes="(max-width: 640px) 100vw, 640px" />

* * *

## Debugging tools for iOS

On iOS we can take advantage of `NSNotificationCenter` to expose the `NSURLRequest`, `NSURLResponse` and the response body to you. In order to check them, you can add the following to your `AppDelegate`:

<pre class="line-numbers"><code class="language-objectivec">- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Set Parse log level to debug to request the debug notifications
    [Parse setLogLevel:PFLogLevelDebug];
    // Register observer and selector to receive the request we sent to server
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(receiveWillSendURLRequestNotification:) name:PFNetworkWillSendURLRequestNotification object:nil];
    // Register observer and selector to receive the response we get from server
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(receiveDidReceiveURLResponseNotification:) name:PFNetworkDidReceiveURLResponseNotification object:nil];
}

- (void)receiveWillSendURLRequestNotification:(NSNotification *) notification {
    // Use key to get the NSURLRequest from userInfo
    NSURLRequest *request = notification.userInfo[PFNetworkNotificationURLRequestUserInfoKey];
}

- (void)receiveDidReceiveURLResponseNotification:(NSNotification *) notification {
    // Use key to get the NSURLRequest from userInfo
    NSURLRequest *request = notification.userInfo[PFNetworkNotificationURLRequestUserInfoKey];
    // Use key to get the NSURLResponse from userInfo
    NSURLResponse *response = notification.userInfo[PFNetworkNotificationURLResponseUserInfoKey];
    NSString *responseBody = notification.userInfo[PFNetworkNotificationURLResponseBodyUserInfoKey];
}</code></pre>

* * *

Once you get the `NSURLRequest`, `NSURLResponse` and the response body, you can check them through the debugger or log them to console in Xcode.

Please check out our Github wiki page ([Android](https://github.com/ParsePlatform/ParseInterceptors-Android/wiki), [iOS](https://github.com/ParsePlatform/Parse-SDK-iOS-OSX/wiki/Network-Debug-Tool)) for more technical details about the network debugging tool. We’d love to hear your feedback.