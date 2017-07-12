---
id: 2414
title: 'Parse Security IV - Ahead in the Cloud'
date: 2014-07-21T16:42:28+00:00
author: Bryan Klimt
layout: post
guid: http://blog.parse.com/?p=2414
permalink: /learn/engineering/parse-security-iv-ahead-in-the-cloud/
dsq_thread_id:
  - "3679015206"
categories:
  - Engineering
---
In our first three posts on how to secure your Parse app, we looked at your app's keys, class-level permissions, and object-level ACLs. For many apps, that's all you need to keep your app and your users' data safe. But sometimes you'll run into an edge case where they aren't quite enough. For everything else, there's <a href="https://parse.com/tutorials/getting-started-with-cloud-code" target="_blank">Cloud Code</a>.

With Cloud Code, you can upload JavaScript to Parse's servers, where we will run it for you. Unlike client code running on users' devices that may have been tampered with, Cloud Code is guaranteed to be the code that you've written, so it can be trusted with more responsibility. For example, if you need to validate the data that a user has entered, you should do it in Cloud Code so that you know a malicious client won't be able to bypass the validation logic. Specifically to help you validate data, Cloud Code has `beforeSave` triggers. These triggers are run whenever an object is saved, and allow you to modify the object, or completely reject a save. For example, this is how you create a Cloud Code trigger to make sure every user has an email address set:

<pre class="brush: javascript; gutter: true">Parse.Cloud.beforeSave(Parse.User, function(request, response) {
  var user = request.object;
  if (!user.get("email")) {
    response.error("Every user must have an email address.");
  } else {
    response.success();
  }
});</pre>

To upload this trigger, follow the instructions in our guide for setting up [the Parse command line tool](https://parse.com/docs/cloud_code_guide#started).

You can also use your master key in Cloud Code to bypass the normal security mechanism for trusted code. For example, if you want to allow a user to “like” a “Post” object without giving them full write permissions on the object, you can do so with a Cloud Code function. Every API function in the Cloud Code JavaScript SDK that talks to Parse allows passing in a `useMasterKey` option. By providing this option, you will use the master key for that one request.

<pre class="brush: javascript; gutter: true">Parse.Cloud.define("like", function(request, response) {
  var post = new Parse.Object("Post");
  post.id = request.params.postId;
  post.increment("likes");
  post.save(null, { useMasterKey: true }).then(function() {
    response.success();
  }, function(error) {
    response.error(error);
  });
});</pre>

One very common use case for Cloud Code is sending push notifications to particular users. In general, clients can't be trusted to send push notifications directly, because they could modify the alert text, or push to people they shouldn't be able to. Your app's settings will allow you to set whether “client push” is enabled or not; we recommend that you make sure it's disabled. Instead, you should write Cloud Code functions that validate the data to be pushed before sending a push.

In this post, we've looked at how you can use Cloud Code to write trusted code, to keep your data secure in cases where class-level permissions and ACLs aren't enough. In Part V, we'll dive into a particular example of how to use ACLs, Roles, and Cloud Code to let users have data that is shared only to their friends.

<span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/06/30/parse-security-i-are-you-the-key-master/" target="_blank">Part I</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/07/07/parse-security-ii-class-hysteria/" target="_blank">Part II</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/07/14/parse-security-iii-are-you-on-the-list/" target="_blank">Part III</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/07/28/parse-security-v-how-to-make-friends/" target="_blank">Part V</a></span>