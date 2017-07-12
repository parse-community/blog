---
id: 3684
title: Open Sourcing Our SDKs
date: 2015-08-13T10:57:48+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parse.com/?p=3684
permalink: /announcements/open-sourcing-our-sdks/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4029705211"
image: /wp-content/uploads/2015/08/open-hero-2b.jpg
categories:
  - Announcements
tags:
  - opensource
  - sdk
---
Ever since Parse joined Facebook, we've been thrilled to be a part of the company's long-standing [commitment](https://code.facebook.com/posts/463284987129903/oscon-2015-how-facebook-open-sources-at-scale/) to open source. It's incredibly exciting to see the widespread success of Facebook open source projects including [React](http://facebook.github.io/react/), [Presto](https://prestodb.io/), [HHVM](http://hhvm.com/), [OSQuery](https://osquery.io/), and [RocksDB](http://rocksdb.org/). Here at Parse, we also believe in open source as a way of accelerating innovation, learning from our community, and collaborating on scalable solutions to hard challenges in mobile development.

That's why we're happy to announce that **we're open sourcing all of our SDKs**, with iOS, OS X, and Android SDKs available today, and our other SDKs soon to come. Our SDKs are widely used by the mobile development community — in fact, Parse SDKs now power more than **800 million active app-device pairs per month**. They're an important part of our platform, enabling developers to get up and running with Parse in minutes. Today we're excited to show developers what's under the hood for the first time.

* * *

As we've built up our SDKs over the years, we've faced many unique challenges that come with building a platform powering your apps across multiple platforms. Here's a few and how we solved them:

### Ease of use

We’ve had to figure out a way to make a public-facing API easy to understand and use, but continue shipping features fast without breaking any existing functionality. To solve this, we structured our public API as a facade for internal code and functionality that could be consistently changing.

### Architecture Unity

In order to achieve unity in architecture we developed a whole new approach to asynchronous operations with 'promises' and Bolts framework `Tasks`, which we [previously](http://blog.parse.com/announcements/lets-bolt/) open sourced.

### Performance

To tackle speed and stability, we also built a loosely coupled architecture model, which lets us move much faster and stay confident in the reliability of our SDKs' existing functionality.

You can read about all this in more detail [here](http://blog.parse.com/learn/the-parse-sdk-whats-inside/).

* * *

Along with open sourcing our SDKs, we're also **opening up our developer support flow**. Our new SDK support flow utilizes GitHub Issues, where you can interact directly with us and other Parse developers. Since the source code is available to all, you'll also be able to submit Pull Requests for any bugs you find. For more on our SDK bug reporting and contribution guidelines, read on here: [Android guidelines](https://github.com/ParsePlatform/Parse-SDK-Android/blob/master/CONTRIBUTING.md), [iOS/OS X guidelines](https://github.com/ParsePlatform/Parse-SDK-iOS-OSX/blob/master/CONTRIBUTING.md).

By sharing our SDK source code with the community, we want to share everything we've learned along the way, as we hope this will benefit others working on similar challenges in the mobile development space. We know we'll continue learning, too, from the contributions of developers and engineers around the world. We can't wait to hear what you have to say.
  
&nbsp;
  
**Ready to dig into the code?**
  
You can find it here for [Android](https://github.com/ParsePlatform/Parse-SDK-Android) and [iOS/OS X](https://github.com/ParsePlatform/Parse-SDK-iOS-OSX).

_Update - September 22, 2015 - We just finished open sourcing all of our SDKs! Here are the repositories for our [JavaScript](https://github.com/ParsePlatform/Parse-SDK-JS) and [.NET](https://github.com/ParsePlatform/Parse-SDK-dotNET) SDKs. Want more? Find out the approaches we took and what we learned in our [Writing Libraries for the Modern Web](http://blog.parse.com/learn/engineering/writing-libraries-for-modern-web/) blog post and [Open Sesame: Parse .NET SDK](http://blog.parse.com/announcements/open-sesame-parse-net-sdk/) blog post!</p>