---
id: 3403
title: Using Parse for IoT to Create an Order Button
date: 2015-04-01T19:09:44+00:00
author: mattieugamacheasselin
layout: post
guid: http://blog.parse.com/?p=3403
permalink: /learn/using-parse-for-iot-to-create-an-order-button/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3687495615"
categories:
  - Engineering
  - Learn
  - Tutorial
tags:
  - arduino
  - hardware
  - IoT
---
Yesterday, Amazon announced the [Dash button](https://www.amazon.com/oc/dash-button), a small device with a single button and a single purpose: Pressing the button orders an item on Amazon. This is just the latest in the continuing trend toward connected smart devices, and we immediately saw the possibilities for our developer community to create devices for their own apps. Feeling inspired to experiment, we spent half an hour with an Arduino Yún, a push button, and the new [Parse for IoT](http://blog.parse.com/2015/03/25/connecting-hardware-with-the-cloud-parse-for-iot/) SDK to create a device that simulates ordering toilet paper when we're running low. As you'll see here, it's amazingly simple to build a working order button that connects to your app using Parse tools. Here's how we did it:

We connected a push button to the Arduino and used a small breadboard so the button would sit neatly on top of the Arduino. As you'll see later, we used the Arduino's packaging as an enclosure.

[<img class="aligncenter size-full wp-image-2767" style="margin: 20px auto; display: block;" src="{{ site.url }}/assets/wp-content/uploads/2015/04/IMG_22661.jpg" alt="IMG_2266" width="800" />]({{ site.url }}/assets/wp-content/uploads/2015/04/IMG_22661.jpg)

The code on the Arduino is as simple as it gets. You can find the full sketch file in the [Github repo](https://github.com/ParsePlatform/AnyButton), but here's the meat of it. It just calls a Cloud Function:

<pre class="line-numbers"><code class="language-javascript">
// Send request to parse from the Arduino Yùn
ParseCloudFunction cloudFunction;
cloudFunction.setFunctionName("orderToiletPaper");
ParseResponse response = cloudFunction.send();
response.close();
</code></pre>

Deployed in Cloud Code, we have this bit of JavaScript. It makes a request to the [Shopify](https://www.shopify.com) store we created, to order some toilet paper.

<pre class="line-numbers"><code class="language-javascript">
// Define the 'orderToiletPaper' Cloud Function
Parse.Cloud.define("orderToiletPaper", function(request, response) {
var shopifyAppId = 'your-shopify-app-id';
var shopifyAppPassword = 'your-shopify-app-password';
var shopifyStoreName = 'your-shopify-store-name';
var shopifyURL = 'https://' + shopifyAppId + ':' + shopifyAppPassword + '@' + shopifyStoreName + '.myshopify.com/admin/orders.json';

Parse.Cloud.httpRequest({
url: shopifyURL,
method: 'POST',
headers: { 'Content-Type' : 'application/json' },
body: {
order: {
email: 'matt@parse.com',
send_receipt: true,
line_items: [
{ title: 'Parse Toilet Paper', id: 443357149, quantity: 1, price: 0.00 }
]
}
}
}).then(function(data) {
response.success('Ordered some TP.');
}, function(error) {
response.error(error);
});
});
</code></pre>

Some of our industrial designers and materials engineers used our state-of-the-art facility to create a truly beautiful product that you can't help but fall in love with. As you can imagine, we used nothing but the latest technologies to create this masterpiece:

[<img class="aligncenter size-full wp-image-2766" style="margin: 20px auto; display: block;" src="{{ site.url }}/assets/wp-content/uploads/2015/04/making.png" alt="making" width="800" />]({{ site.url }}/assets/wp-content/uploads/2015/04/making.png)

The next time we're running out of toilet paper, you can be sure it will be promptly refilled with another [cloud-soft roll](http://parse-for-iot.myshopify.com/).

<img class="aligncenter size-full wp-image-2768" style="margin: 20px auto; display: block;" src="{{ site.url }}/assets/wp-content/uploads/2015/04/IMG_2272.jpg" alt="IMG_2272" width="800" />

Taking this example further, you could associate devices with user accounts, fully integrate with Shopify or another API (or build your own!) and offer a premium push-button experience for people using your app. You can find the full source on our [Github repository](https://github.com/ParsePlatform/AnyButton), but most of it is already in this blog post. The whole app is less than 100 lines of code!

We're excited about the possibilities of Parse for IoT, including order buttons and much more. Let us know what you build!