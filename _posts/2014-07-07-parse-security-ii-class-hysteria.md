---
id: 2410
title: 'Parse Security II - Class Hysteria!'
date: 2014-07-07T16:28:18+00:00
author: bryanklimt
layout: post
guid: http://blog.parseplatform.org/?p=2410
permalink: /learn/engineering/parse-security-ii-class-hysteria/
dsq_thread_id:
  - "3687646155"
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Engineering
---
_**Update: With pointer permissions, CLPs just got a lot more powerful, and they can now handle a lot more use cases! Check out our <a href="http://blog.parseplatform.org/learn/secure-your-app-from-the-data-browser-with-pointer-permissions/" target="_blank">blog post</a> and <a href="https://parse.com/docs/ios/guide#security-pointer-permissions" target="_blank">docs</a>.**_

In <a href="http://blog.parseplatform.org/2014/06/30/parse-security-i-are-you-the-key-master/" target="_blank">Part I</a>, we took a look at the different keys that a Parse app has and what they mean. As we learned, your client keys are easily accessible to anybody, so you need to rely on Parse's security features to lock down what the client key is allowed to do.

The easiest way to lock down your app is with class-level permissions. Almost every class that you create should have these permissions tweaked to some degree. For classes where every object has the same permissions, class-level settings will be most effective. For example, one common use case: having a class of static data that can be read by anyone but written by no one. If you need different objects to have different permissions, you'll have to use ACLs--Access Control Lists--(discussed in Part III). To edit your class-level permissions, click on "Set permissions" under the "More" menu in the Data Browser for the class you want to configure.

[<img class="aligncenter size-full wp-image-2418" src="{{ site.url }}/assets/wp-content/uploads/2014/06/Screen-Shot-2014-06-30-at-2.54.56-PM.png" alt="Set permissions menu" width="512" height="350" />]({{ site.url }}/assets/wp-content/uploads/2014/06/Screen-Shot-2014-06-30-at-2.54.56-PM.png)

For each setting, you can choose to either leave it open to everyone, or to restrict it to certain predefined Roles or Users. A Role is simply a set of Users and Roles who should share some of the same permissions. For example, you can set up a Role called “admins” and make a table writeable only by that role.

[<img class="aligncenter size-full wp-image-2411" src="{{ site.url }}/assets/wp-content/uploads/2014/06/Data_Browser___Parse.png" alt="Class-level Permissions" width="928" height="668" />]({{ site.url }}/assets/wp-content/uploads/2014/06/Data_Browser___Parse.png)

Let's go over what each permission means.

<ul class="standard-list">
  <li>
    <strong>Get</strong> - With Get permission, users can fetch objects in this table if they know their objectIds.
  </li>
  <li>
    <strong>Find</strong> - Anyone with Find permission can query all of the objects in the table, even if they don't know their objectIds. Any table with public Find permission will be completely readable by the public, unless you put an ACL on each object.
  </li>
  <li>
    <strong>Update</strong> - Anyone with Update permission can modify the fields of any object in the table that doesn't have an ACL. For publicly readable data, such as game levels or assets, you should disable this permission.
  </li>
  <li>
    <strong>Create</strong> - Like Update, anyone with Create permission can create new objects of a class. As with the Update permission, you'll want to turn this off for publicly readable data.
  </li>
  <li>
    <strong>Delete</strong> - This one should be pretty obvious. With this permission, people can delete any object in the table that doesn't have an ACL. All they need is its objectId.
  </li>
  <li>
    <strong>Add fields</strong> -This is probably the strangest permission. Parse classes have schemas that are inferred when objects are created. While you're developing your app, this is great, because you can add a new field to your object without having to make any changes on the backend. But once you ship your app, it's very rare to need to add new fields to your classes automatically. So you should pretty much always turn off this permission for all of your classes when you submit your app to the public.
  </li>
</ul>

There's also an app-level permission to disable client class creation. You should turn this off for production app so that clients can't create new classes. Usually this isn't a big security risk, but it's nice to stop malicious people from creating new tables that you'll see in the data browser.

[<img class="aligncenter size-full wp-image-2412" src="{{ site.url }}/assets/wp-content/uploads/2014/06/Screen-Shot-2014-06-26-at-10.27.31-AM.png" alt="Client Class Creation" width="685" height="207" />]({{ site.url }}/assets/wp-content/uploads/2014/06/Screen-Shot-2014-06-26-at-10.27.31-AM.png)

Class-level permissions are great for globally-shared data where permissions for each object in the class should be exactly the same. In Part III, we'll learn about ACLs, which allow you to have different permissions for each object in a class.

<span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/06/30/parse-security-i-are-you-the-key-master/" target="_blank">Part I</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/14/parse-security-iii-are-you-on-the-list/" target="_blank">Part III</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/21/parse-security-iv-ahead-in-the-cloud/" target="_blank">Part IV</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/28/parse-security-v-how-to-make-friends/" target="_blank">Part V</a></span>