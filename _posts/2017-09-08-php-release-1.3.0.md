---
layout: post
title: Parse PHP SDK Release 1.3.0
date: 2017-09-08 11:02 -0700
comments: true
author: montymxb
categories: [PHP, SDK, Release]
---

Version 1.3.0 of the Parse PHP SDK is [now available](https://github.com/parse-community/parse-php-sdk/releases/tag/1.3.0) for use.
This is a jump from **1.2.10** which now includes support for HHVM, full text query support and more!

<!-- more -->

The major changes are as follows:

- Adds **HHVM** support

- Modifies `ParseFile` to use the current **HttpClient** rather than just curl for download

- Adds full text search via `ParseQuery::fullText` for Parse Server **2.5.0** and later

- Adds **encode**/**decode** support to `ParseObject`

- Travis CI cache fixes

- Slight test modifications for later versions of parse

- Some general typos and documentation additions

A more detailed breakdown of the differences between **1.2.10** and **1.3.0** can be seen [here](https://github.com/parse-community/parse-php-sdk/compare/1.2.10...1.3.0).
We hope that the inclusion of support for HHVM will allow developers a bit more freedom in their setup when using the php sdk.
Additionally the changes to `ParseFile` are to address any issues running on a cURL-less environment, where downloading may have been non-functional (as it was hard-coded to use curl rather than the current **HttpClient**).

The addition of full text support (thanks to [@dplewis](https://github.com/dplewis)) also expands upon the available methods by which to query with.
Using a full text search in a query takes advantage of text indexes server side to help speed up the request, especially for rather large datasets.

Encoding and Decoding of ParseObjects is a new feature that effectively allows you to serialize an object, in place, as JSON.
Not only is this easy to work with and to inspect, but any encoded objects can later be decoded. Creating a means of storing away objects (with unsaved changes preserved) and later decoding them to inspect or use.

We hope to see developers taking advantage of these new features and to let us know what they think! We're always looking to improve things.
If you have a thought or concern drop us a line or open an issue over on [github](https://github.com/parse-community/parse-php-sdk), we're always around!
