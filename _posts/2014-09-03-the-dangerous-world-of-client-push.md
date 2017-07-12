---
id: 2494
title: The Dangerous World of Client Push
date: 2014-09-03T19:08:02+00:00
author: Jamie Karraker
layout: post
guid: http://blog.parse.com/?p=2494
permalink: /learn/engineering/the-dangerous-world-of-client-push/
post_format:
  - basic
dsq_thread_id:
  - "3678687810"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Engineering
tags:
  - push
  - security
---
<p class="p1">
  Push Notifications are one of the most effective ways for apps to increase user engagement and retention, notify customers of important information like sales and new products, or allow users communicate with each other. A messaging app, for example, could use push notifications to alert users to an incoming message from their friend in the app. But be careful, because the most obvious way to do this can expose you to a security risk.
</p>

<p class="p1">
  One easy way to send push notifications to particular users from within an app is to send the pushes directly from the client code, for example by using <code>PFPush</code> on iOS. This is tempting, because you have all the information you need to send the push straight from within iOS, and the code to send the push is very simple. But as Bryan mentioned in his <a title="Parse Security IV – Ahead in the Cloud" href="http://blog.parse.com/2014/07/21/parse-security-iv-ahead-in-the-cloud/" target="_blank">security series</a>, clients can’t be trusted to send push notifications directly. Some nefarious hacker could modify the client code, changing the Objective-C or Java code to modify the push's alert text, or to send pushes to people they shouldn’t be able to. The hacker could steal your app's keys and send pushes himself without even using the app.
</p>

<p class="p1">
  Sounds scary! Luckily, there is a simple way around this security vulnerability. In your app's settings, under "Push Notifications", there is a toggle that lets you disable "Client Push." We highly recommend that you set this switch to "OFF," and instead of sending the push notifications from the client, you should write <a href="http://parse.com/docs/cloud_code_guide">Cloud Code</a> functions that validate the user and the data to be pushed before sending them. Client Push is disabled by default, but this feature can still be useful for initial prototyping if you want to hook up push notifications without diving into Cloud Code as you iterate. But as you scale to a production app, you really should not have Client Push enabled.
</p>

<p class="p1">
  <a href="{{ site.url }}/assets/wp-content/uploads/2014/08/Screenshot-2014-08-28-21.28.261.png"><img class="alignnone wp-image-2496" src="{{ site.url }}/assets/wp-content/uploads/2014/08/Screenshot-2014-08-28-21.28.261-1024x241.png" alt="" width="1000" height="236" /></a>
</p>

<p class="p1">
  Here is one example of how to port your pushes from the client into Cloud Code, starting with an example Client Push to a single user, stored in <code>userObject</code>. Here's the old iOS code:
</p>

<pre class="brush: objc; gutter: false">// WRONG WAY TO SEND PUSH - INSECURE!
PFQuery *pushQuery = [PFInstallation query];
[pushQuery whereKey:@"user" equalTo:userObject];
PFUser *user = [PFUser currentUser];
NSString *message = [NSString stringWithFormat:@"%@ says Hi!", user[@"name"]];

PFPush *push = [[PFPush alloc] init];
[push setQuery:pushQuery]; // Set our Installation query
[push setMessage:message];
[push sendPushInBackground];</pre>

<p class="p1">
  And on Android:
</p>

<pre class="brush: java; gutter: false">// WRONG WAY TO SEND PUSH - INSECURE!
ParseQuery pushQuery = ParseInstallation.getQuery();
pushQuery.whereEqualTo("user", userObject);
ParseUser currentUser = ParseUser.getCurrentUser();
String message = currentUser.getString("name") + " says Hi!";

ParsePush push = new ParsePush();
push.setQuery(pushQuery); // Set our Installation query
push.setMessage(message);
push.sendInBackground();</pre>

<p class="p1">
  To move these push calls to Cloud Code, first we would write a <a href="http://parse.com/docs/cloud_code_guide#functions">Cloud function</a>, <code>sendPushToUser</code>, to verify that the user sending the push is allowed to send it and that the alert text is what it should be.
</p>

<pre class="brush: javascript; gutter: true">Parse.Cloud.define("sendPushToUser", function(request, response) {
  var senderUser = request.user;
  var recipientUserId = request.params.recipientId;
  var message = request.params.message;

  // Validate that the sender is allowed to send to the recipient.
  // For example each user has an array of objectIds of friends
  if (senderUser.get("friendIds").indexOf(recipientUserId) === -1) {
    response.error("The recipient is not the sender's friend, cannot send push.");
  }

  // Validate the message text.
  // For example make sure it is under 140 characters
  if (message.length &gt; 140) {
  // Truncate and add a ...
    message = message.substring(0, 137) + "...";
  }

  // Send the push.
  // Find devices associated with the recipient user
  var recipientUser = new Parse.User();
  recipientUser.id = recipientUserId;
  var pushQuery = new Parse.Query(Parse.Installation);
  pushQuery.equalTo("user", recipientUser);
 
  // Send the push notification to results of the query
  Parse.Push.send({
    where: pushQuery,
    data: {
      alert: message
    }
  }).then(function() {
      response.success("Push was sent successfully.")
  }, function(error) {
      response.error("Push failed to send with error: " + error.message);
  });
});</pre>

<p class="p1">
   After you <a title="Cloud Code" href="https://parse.com/docs/cloud_code_guide#started-simple" target="_blank">deploy</a> this Cloud function, you can call the code from iOS like this:
</p>

<pre class="brush: objc; gutter: false">[PFCloud callFunctionInBackground:@"sendPushToUser"
                   withParameters:@{@"recipientId": userObject.id, @"message": message}
                            block:^(NSString *success, NSError *error) {
  if (!error) {
     // Push sent successfully
  }
}];</pre>

<p class="p1">
  And on Android:
</p>

<pre class="brush: java; gutter: false">HashMap&lt;String, Object&gt; params = new HashMap&lt;String, Object&gt;();
params.put("recipientId", userObject.getObjectId());
params.put("message", message);
ParseCloud.callFunctionInBackground("sendPushToUser", params, new FunctionCallback&lt;String&gt;() {
   void done(String success, ParseException e) {
       if (e == null) {
          // Push sent successfully
       }
   }
});</pre>

<p class="p1">
  Now that you have moved the Push logic into Cloud Code, you can disable Client Push in your app settings, and spoil the hacker's evil plan! We hope you learned how to use Cloud Code to secure your app's push notifications, and that you will think twice before enabling Client Push for your app. As always, feel free to ask questions or let us know what you think on our <a title="Help" href="https://parse.com/help" target="_blank">new Help page</a>.
</p>