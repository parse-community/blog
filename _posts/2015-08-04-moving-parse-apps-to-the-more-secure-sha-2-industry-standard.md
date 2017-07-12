---
id: 3659
title: Moving Parse Apps to the More Secure SHA-2 Industry Standard
date: 2015-08-04T12:09:47+00:00
author: StanleyWang
layout: post
guid: http://blog.parse.com/?p=3659
permalink: /announcements/moving-parse-apps-to-the-more-secure-sha-2-industry-standard/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4003214107"
categories:
  - Announcements
tags:
  - security
  - SHA-2
  - ssl
---
Over the past few years, the internet industry has been undertaking a shift from SHA-1 to SHA-2 certificates for encryption. It's widely known that certificates with SHA-2 signatures are more secure than the older SHA-1 certificates, and many companies have already begun phasing out support for SHA-1, including [Google](http://blog.chromium.org/2014/09/gradually-sunsetting-sha-1.html), [Apple](https://developer.apple.com/library/prerelease/ios/technotes/App-Transport-Security-Technote/index.html), [Microsoft](http://blogs.technet.com/b/pki/archive/2013/11/12/sha1-deprecation-policy.aspx), and [Facebook](https://developers.facebook.com/blog/post/2015/06/02/SHA-2-Updates-Needed/). The upcoming iOS 9 release will require SHA-2 certificates.

In order to prepare Parse to support iOS 9 apps, and to help our developers build more secure apps, we plan to upgrade the SSL certificate for https://api.parse.com to SHA-2 on **Tuesday, August 11, 2015**.

* * *

## Clients That are not Impacted 

**This does not impact phones and web browsers that were released in the past few years** (e.g. Android 2.3+, iOS 3.0+, Windows Phone 7+, Chrome 26+, Firefox 1.5+, IE 6+ with XP SP3+). These modern clients are already built to handle SHA-2 certificates. Therefore, the number of people running unsupported clients should be minimal. For example, [only 0.3% of the overall Android user base](https://developer.android.com/about/dashboards/index.html) is running a version that would be impacted by this upgrade. You can find a list of SHA-2 compatible clients on [DigiCert's website](https://www.digicert.com/sha-2-compatibility.htm).

## Clients That may Have Issues 

If you're **calling the [Parse REST API](https://www.parse.com/docs/rest/guide) directly using an outdated HTTP client or accessing Parse from an Internet-of-Things (IoT) device**, we strongly recommend that you test your HTTP client's SHA-2 compatibility by making a request to https://sha2-test.parse.com from your client. Your HTTP request should be the equivalent of this CURL command:

<pre class="line-numbers"><code class="language-bash">curl -X GET https://sha2-test.parse.com</code></pre>

If your HTTP client is SHA-2 compatible, you should get back this JSON response:

<pre class="line-numbers"><code class="language-bash">{ "test": 1 }</code></pre>

If your HTTP client is unable to connect to this URL due to an SSL error, please be sure to update your client by **August 11th**. Incompatible clients will not be able to connect to Parse on or after August 11th. You may need to upgrade its HTTP libraries (e.g. OpenSSL) or install new SSL root certificates. Please refer to your HTTP client's documentation for details.

## Feedback

We also want to better understand the impact of this change for our developers. If you find that your client is not SHA-2 compatible, please comment on [this bug](https://developers.facebook.com/bugs/890792687654337/) with details about your HTTP client.

* * *

We are always working to make Parse more secure. In addition to improving our overall platform, we also provide several tools to help you manage and secure your app's data. You can learn more about them [here](http://blog.parse.com/tags/security/).