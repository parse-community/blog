---
id: 3558
title: Make a Cloud-Controlled Car using Parse for IoT
date: 2015-05-20T12:19:58+00:00
author: ronaldyang
layout: post
guid: http://blog.parse.com/?p=3558
permalink: /learn/make-a-cloud-controlled-car-using-parse-for-iot/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3780667477"
categories:
  - Learn
  - Tutorial
tags:
  - hardware
  - IoT
  - vroom
---
If you walk into a model shop or toy store today, you'll probably see a wide variety of remotely-controlled toy cars available. Almost all of them are RC-based (radio control), meaning you'll lose control when outside of the radio range.

<img class="alignnone wp-image-3559 size-medium" style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-1-300x286.png" alt="Cloud Car 1" width="300" height="286" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-1-300x286.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-1.png 535w" sizes="(max-width: 300px) 100vw, 300px" />

But with a cloud-controlled car, you can control it from anywhere in the world. In this post, we'll walk you through the steps of how to build this fun toy using Parse's [tools for connected devices](http://blog.parse.com/learn/connecting-hardware-with-the-cloud-parse-for-iot/) (and you can find the full source code at the bottom of the post). After reading this, we hope you can build your own cloud-controlled car with much less engineering effort than ever before — or maybe this will inspire you to build something even more amazing.

Let's hit the road:

* * *

## Step 1: What do you need?

<ul class="standard-list">
  <li>
    <b>Car body </b>(wheels, chassis frame, motors, motor circuits): For our car body, we've adapted a <a href="http://www.amazon.com/Super-4-wheel-Chassis-Encoder-Arduino/dp/B00VT1NFP6">4-wheel Robot Smart Car Chassis Kit</a> (including four wheels, chassis, and four motors). We handcrafted the motor circuits using an <a href="http://www.amazon.com/gp/product/B00NAY2URO/">H-bridge</a>. As a handy alternative option for motor circuits, you could use a <a href="https://www.adafruit.com/products/1438">Motor Shield</a>, but in this post we'll see how we can use simple circuits to drive the car. You'll also need a <a href="http://www.amazon.com/microtivity-IB401-400-point-Experiment-Breadboard/dp/B004RXKWDQ">bread board and a bunch of wires</a>. We won't cover too much mechanical/electrical engineering here, but there you have the basics.
  </li>
  <li>
    <b>Control system</b>: We used the <a href="https://www.parse.com/apps/quickstart#embedded/ticc3200">TI CC3200</a> as the controlling device, although <a href="https://www.parse.com/apps/quickstart#embedded/arduinoyun">Arduino</a>, <a href="https://www.parse.com/apps/quickstart#embedded/raspberrypi">Raspberry Pi</a>, and Intel Edison are also good options. Four GPIO pins in CC3200 are connected with the motor circuits described above.
  </li>
  <li>
    <b>Power</b>: 9V battery for powering the motors, and a portable USB charger for powering the CC3200.
  </li>
</ul>

<div style="text-align: center;">
  <img class="alignnone wp-image-3560" src="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-2-225x300.png" alt="Cloud Car 2" width="244" height="325" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-2-225x300.png 225w, {{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-2.png 443w" sizes="(max-width: 244px) 100vw, 244px" />
</div>

* * *

## 

## Step 2: Build the body for the car

First let us assemble the car body template including chassis, wheels and motors.

Then we focus on building the motor circuits. If you look at the [datasheet of L293D IC](https://github.com/sudar/DCMotorBot/blob/master/datasheet/l293d.pdf?raw=true), you'll find that we can control two motors simultaneously using the IC. Even though the IC operates at 5V, it can still give a higher voltage (up to 36V) to the motor.

Here's the pin diagram of the IC:

<div style="text-align: center;">
  <img class="alignnone wp-image-3561" src="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-3.png" alt="Cloud Car 3" width="327" height="175" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-3.png 413w, {{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-3-300x161.png 300w" sizes="(max-width: 327px) 100vw, 327px" />
</div>

As explained above, we'll be connecting a H-bridge to a CC3200, using the following connection.

<div style="text-align: center;">
  <img class="alignnone wp-image-3562" src="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-4-875x694.png" alt="Cloud Car 4" width="631" height="500" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-4-875x694.png 875w, {{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-4-300x238.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-4.png 879w" sizes="(max-width: 631px) 100vw, 631px" />
</div>

### Controlling direction of DC Motors

As you can see from the pin diagram above, each motor has two control pins through which we can control the motor. Two motors in one side will be given same inputs, which means they'll always rotate in same direction. The logic to control the direction of the motor is as follows:

<ul class="standard-list">
  <li>
    If control pin 1 is HIGH and control pin 2 is LOW, then the motor will rotate in one direction.
  </li>
  <li>
    If control pin 1 is LOW and control pin 2 is HIGH, then the motor will rotate in the other direction.
  </li>
</ul>

* * *

## 

## Step 3: Device programming using the Parse Embedded Devices SDK

The code on the CC3200 is responsible for establishing a push channel with the Parse Cloud. It interprets the received commands into electrical signals sent to GPIO pins.

Four GPIO pins are configured, and each pair of pins are controlling two wheels on one side.

<pre class="line-numbers"><code class="language-csharp">&gt;// Configure PIN_16/18/61/62 for GPIO Output
MAP_PinTypeGPIO(PIN_16, PIN_MODE_0, false);
MAP_GPIODirModeSet(GPIOA2_BASE, 0x80, GPIO_DIR_MODE_OUT);
MAP_PinTypeGPIO(PIN_18, PIN_MODE_0, false);
MAP_GPIODirModeSet(GPIOA3_BASE, 0x10, GPIO_DIR_MODE_OUT);
MAP_PinTypeGPIO(PIN_61, PIN_MODE_0, false);
MAP_GPIODirModeSet(GPIOA0_BASE, 0x40, GPIO_DIR_MODE_OUT);
MAP_PinTypeGPIO(PIN_62, PIN_MODE_0, false);
MAP_GPIODirModeSet(GPIOA0_BASE, 0x80, GPIO_DIR_MODE_OUT);</code></pre>

<pre class="line-numbers"><code class="language-csharp">
#define LEFT_CONTROL_PIN1 6
#define LEFT_CONTROL_PIN2 7
#define RIGHT_CONTROL_PIN1 23
#define RIGHT_CONTROL_PIN2 28</code></pre>

When a “drive” command is received, the code will set **LEFT\_CONTROL\_PIN1**, ****RIGHT\_CONTROL\_PIN1**** to low voltage and **LEFT\_CONTROL\_PIN2**,** **RIGHT\_CONTROL\_PIN2**** to high voltage, triggering the motors to rotate in one direction. This works in vice-versa for the “reverse” command.

<pre class="line-numbers"><code class="language-csharp">if (strcmp("drive", msgType) == 0) {
  GPIO_IF_Set(**LEFT_CONTROL_PIN1**, PORT_LC1, GPIO_PIN_LC1, **0**);
  GPIO_IF_Set(**LEFT_CONTROL_PIN2**, PORT_LC2, GPIO_PIN_LC2, **1**);
  GPIO_IF_Set(**RIGHT_CONTROL_PIN1**, PORT_RC1, GPIO_PIN_RC1, **0**);
  GPIO_IF_Set(**RIGHT_CONTROL_PIN2**, PORT_RC2, GPIO_PIN_RC2, **1**);

  UtilsDelay(period); // 1000 ms

  GPIO_IF_Set(LEFT_CONTROL_PIN1, PORT_LC1, GPIO_PIN_LC1, 0);
  GPIO_IF_Set(LEFT_CONTROL_PIN2, PORT_LC2, GPIO_PIN_LC2, 0);
  GPIO_IF_Set(RIGHT_CONTROL_PIN1, PORT_RC1, GPIO_PIN_RC1, 0);
  GPIO_IF_Set(RIGHT_CONTROL_PIN2, PORT_RC2, GPIO_PIN_RC2, 0);

} else if (strcmp("reverse", msgType) == 0) {
  GPIO_IF_Set(**LEFT_CONTROL_PIN1**, PORT_LC1, GPIO_PIN_LC1, **1**);
  GPIO_IF_Set(**LEFT_CONTROL_PIN2**, PORT_LC2, GPIO_PIN_LC2, **0**);
  GPIO_IF_Set(**RIGHT_CONTROL_PIN1**, PORT_RC1, GPIO_PIN_RC1, **1**);
  GPIO_IF_Set(**RIGHT_CONTROL_PIN2**, PORT_RC2, GPIO_PIN_RC2, **0**);

  UtilsDelay(period); // 1000 ms

  GPIO_IF_Set(LEFT_CONTROL_PIN1, PORT_LC1, GPIO_PIN_LC1, 0);
  GPIO_IF_Set(LEFT_CONTROL_PIN2, PORT_LC2, GPIO_PIN_LC2, 0);
  GPIO_IF_Set(RIGHT_CONTROL_PIN1, PORT_RC1, GPIO_PIN_RC1, 0);
  GPIO_IF_Set(RIGHT_CONTROL_PIN2, PORT_RC2, GPIO_PIN_RC2, 0);
}</code></pre>

We're not showing the device code in its entirety here, but the structure follows the [Blink example](https://github.com/ParsePlatform/parse-embedded-sdks/tree/master/cc3200/samples/blink), with the above code being called from the push notifications handler.

* * *

## 

## Step 4: Set up your Cloud Code

Deployed in Cloud Code, we have a JavaScript function here:

<pre class="line-numbers"><code class="language-javascript">Parse.Cloud.afterSave("Message", function(request, response) {
  var installationId = request.object.get("installationId");
  var query = new Parse.Query(Parse.Installation);
  if (installationId !== null && installationId !== undefined) {
    query.equalTo("installationId", installationId);
  } else {
    return;
  }

  Parse.Push.send({
    where: query,
    data: request.object.get("value"),
  },
  {
    success: function() {
      console.log("sending push to device");
    },
    error: function(err) {
      console.log(err);
    }
  });
});</code></pre>

A custom class, “Message,” should be created after deploying the Cloud Code. When a Message object is saved, this cloud function sends a push to the device, and that has the same installationId as in the Message object saved. That being said, saving an object like

<pre class="line-numbers"><code class="language-csharp">{"installationId":"YOUR_CAR_INSTALLATION_ID","value":{"alert": "drive"}}</code></pre>

will tell Parse to send a drive command to the car.

* * *

## 

## Step 5: Play with the car!

Now we can put our wheels to the test. We prototyped two ways to control the car. <img class="alignnone size-full wp-image-3563" style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-5.png" alt="Cloud Car 5" width="287" height="509" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-5.png 287w, {{ site.url }}/assets/wp-content/uploads/2015/05/Cloud-Car-5-169x300.png 169w" sizes="(max-width: 287px) 100vw, 287px" />

<ul class="standard-list">
  <li>
    <b>Control with mobile: </b>We've developed an iOS app you can use to provisioning your car (create Parse installation object, configure WiFi) and send a control command remotely. The Android version is also available upon request.
  </li>
</ul>

<ul class="standard-list">
  <li>
    <b>Control with speech</b>: <a href="http://wit.ai">wit.ai</a> provides a great speech-to-intent service, which enables us to send a voice control command. Check out this demo <a href="https://www.facebook.com/siyinyang/videos/vb.1332893165/10204372010483685/">video</a>.
  </li>
</ul>

And that's all there is to it! You can find the full code in this [Github repo](https://github.com/ParsePlatform/InternetCar). We'd love to hear your feedback, and please let us know what amazing things you've built with Parse for IoT.