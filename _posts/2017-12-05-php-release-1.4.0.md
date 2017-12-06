---
layout: post
title: Parse PHP SDK Release 1.4.0
date: 2017-12-05 20:24 -0700
comments: true
author: montymxb
categories: [PHP, SDK, Release, 1.4.0]
---

Version 1.4.0 of the Parse PHP SDK is [now available](https://github.com/parse-community/parse-php-sdk/releases/tag/1.4.0) for use.
This is a jump from **1.3.0** which now includes quite a few new features. Among them being relative time queries, aggregate queries and index management.

<!-- more -->

Release 1.4.0 contains quite a few new features that we hope you'll enjoy! Please keep in mind that relative time queries, aggregate queries and index management are only available in later versions of parse server (versions **2.6.5** and above, and versions **2.7.0** and above for index management). The other features present here have been available in parse server for some time and have now been hooked up into this sdk.

Version **1.4.0** is reverse compatible with **1.3.0**, so you're good for upgrading 👍 . Just keep in mind that some new features _may not_ be available depending on your version of parse server. You can check your version of parse server from this sdk using ```ParseServerInfo::getVersion``` to check if you don't know for sure (version checking is compatible with older versions of parse server).

All new features have been added into the [Table of Contents in our README](https://github.com/parse-community/parse-php-sdk#table-of-contents) for your reading convenience.

Here are some of the major changes that were made in this release.

- Adds Relative Time Queries
- Adds Server Info
- README and code cleanup, adds **CHANGELOG** and **CODE_OF_CONDUCT**
- Adds Purge & Polygon to ParseSchema
- Adds Parse Server Health Check
- Adds ability to Request Verification Emails
- Adds the ability to set/save in `ParseConfig` 
- Adds `ParseLogs`
- Adds `ParseAudience`
- Adds jobs to `ParseCloud`
- Adds support for aggregate queries (thanks to [Diamond Lewis](https://github.com/dplewis))
- Support for managing indexes via **ParseSchema** (thanks to [Diamond Lewis](https://github.com/dplewis))

You can find more detail regarding these changes on the [1.4.0 release page](https://github.com/parse-community/parse-php-sdk/releases/tag/1.4.0).
As always we hope you enjoy these new features. If you have any questions feel free to reach out to us on [github](https://github.com/parse-community/parse-php-sdk) and let us know what you think. 
We're always open to improving parse server and it's sdks for the community!