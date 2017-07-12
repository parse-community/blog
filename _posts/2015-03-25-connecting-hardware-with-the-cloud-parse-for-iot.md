---
id: 3394
title: 'Connecting Hardware with the Cloud: Parse for IoT'
date: 2015-03-25T10:51:44+00:00
author: James Yu
layout: post
guid: http://blog.parse.com/?p=3394
permalink: /learn/connecting-hardware-with-the-cloud-parse-for-iot/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3687491701"
image: /wp-content/uploads/2015/04/things-blog.jpg
categories:
  - Announcements
  - Engineering
  - Learn
tags:
  - arduino
  - hardware
  - IoT
---
At Parse, our passion is making developer experiences easier on any platform—including platforms that extend beyond mobile. Of these platforms, one of the most exciting new spaces is the Internet of Things. We believe that connecting more hardware devices with the cloud has the potential to change the world for the better. We are already seeing devices that add tremendous value to people's lives, from wearables that help you sleep better to insulin trackers that aid people living with diabetes.

But, as with mobile, connecting these devices to the cloud can be difficult. In addition to maintaining a backend, developers must contend with notoriously constrained environments on the client. We've been listening to feedback from a wide range of Parse customers who are already using our platform in hardware products — like Chamberlain, who makes a line of smart garage door openers that interact with our REST API; Milestone Sports, who make the wearable running tracker Milestone Pod; and Roost, who make smart batteries for smoke detectors. From these conversations, we decided we could go one step further.

Today, we're proud to announce Parse for IoT: an official new line of SDKs for connected devices.

The first is an Arduino SDK targeted for the [Arduino Yún](http://arduino.cc/en/Main/ArduinoBoardYun?from=Products.ArduinoYUN), a microcontroller board with built-in WiFi capabilities. The SDK interface is in Wiring, and, in the spirit of Arduino, we designed it to be as simple as possible. For example, all it takes is a few lines of code to save temperature data from a smart thermostat:

<pre class="line-numbers"><code class="language-c">ParseCreateObject create;
create.setClassName('TemperatureReading');
create.add('currentTemperature', 175.0);
create.send();</code></pre>

From there, the data will be available in your Parse app ready to be retrieved by your mobile app, another device, or simply logged for analytics purposes. Beyond the [Yún,](http://arduino.cc/en/Main/ArduinoBoardYun?from=Products.ArduinoYUN) we're already working on SDKs for upcoming platforms such as the Arduino Zero with the WiFi 101 shield.

In addition, we're releasing an Embedded C SDK, targeted for Linux and Real Time Operating Systems (RTOS). These open source SDKs serve as reference implementations that are being used by leading chipset manufacturers to provide support for their hardware platforms. _If you are a chipset manufacturer interested in working with us, please reach out at <iotpartners@fb.com>._

The C SDK provides a simple interface for our REST API. For example, to save the same temperature data you would do:

<pre class="line-numbers"><code class="language-c">char data[] = '{ \'currentTemperature\': 175.0 }';
parseSendRequest(client, 'POST', '/1/classes/TemperatureReading', data, NULL);</code></pre>

You'll be able to find these SDKs on [GitHub](https://github.com/ParsePlatform/parse-embedded-sdks), as well as a full set of Quick Starts and Guides on Parse. With these SDKs, your device will be able to receive push notifications, save data, and take advantage of the Parse Cloud. It's easy to get started from scratch, and the process should be very familiar for developers who already use Parse. Check out the [Quick Start guide](https://parse.com/apps/quickstart#embedded) and start adding Parse to your hardware device in minutes.

The possibilities are endless. You could make a smart thermostat that can be controlled via a mobile app, or a security camera that saves images every minute, or even a music device that can be controlled via a web app. We're so excited to see what you build.