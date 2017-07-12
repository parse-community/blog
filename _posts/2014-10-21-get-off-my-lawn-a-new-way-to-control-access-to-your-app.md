---
id: 2572
title: 'Get Off My Lawn: a New Way to Control Access to Your App'
date: 2014-10-21T18:35:56+00:00
author: Jamie Karraker
layout: post
guid: http://blog.parse.com/?p=2572
permalink: /learn/get-off-my-lawn-a-new-way-to-control-access-to-your-app/
post_format:
  - basic
dsq_thread_id:
  - "3678964346"
categories:
  - Learn
---
Security plays an important part in releasing an app on Parse, and [Access Control Lists (ACLs)](https://parse.com/docs/data#security-objects) are one of the most powerful ways to secure your app. As Bryan mentioned in [part III of his security series](http://blog.parse.com/2014/07/14/parse-security-iii-are-you-on-the-list/), “If you have a production app and you aren’t using ACLs, you’re almost certainly doing it wrong.” In that blog post he examines how to create and save ACLs in code. Another effective way to create and manage ACLs is through the [data browser](https://parse.com/docs/data#data-browser), and today that is much easier with a new and improved ACL editor. The data browser is already one of our most popular and most frequently used tools, and now you can also use the data browser to easily edit and manage your app's ACLs.

An ACL is essentially a list of users that are allowed to read or write a Parse object. For example, let's say your social networking app has a `Post` object, and you want the user who created that post, `jack`, to be the only person who can see it. You would then create an ACL on that object and give Jack read permissions. By default, ACLs are public read and write, so to add an ACL on Jack, first uncheck public read and write, and then type Jack's username into the ACL editor. Each step is shown in the new editor, along with the equivalent code in iOS.

[<img class="alignnone size-large wp-image-2573" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.48.56-1024x801.png" alt="Screenshot 2014-10-20 19.48.56" width="584" height="456" />]({{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.48.56.png)

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">PFUser *jack = [PFUser currentUser]; // username "jack"
PFObject *post = [PFObject objectWithClassName:@"Post"];
PFACL *acl = [PFACL ACL];
[acl setReadAccess:true forUser:jack];</pre>

Now Jack's post is private, and Jack is the only one who can see it. But nobody has write permissions on the object, so its fields can only be changed by using the [master key](http://blog.parse.com/2014/06/30/parse-security-i-are-you-the-key-master/). What if Jack wants to edit his post? To allow this, you need to give Jack write permissions in the object's ACL.

`<a href="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.00.png"><img class="alignnone size-large wp-image-2574" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.00-1024x799.png" alt="Screenshot 2014-10-20 19.49.00" width="584" height="455" /></a>`

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[acl setWriteAccess:true forUser:jack];</pre>

Now Jack can both see and edit his Post object. But what if Jack wants to share his Post with his friend `jill`? Right now Jack is the only user with read or write permissions on the object. You want to allow Jill to be able to see the post, but you don't want her to be able to edit it. Therefore you need to add read permissions for Jill on the ACL.

`<a href="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.08.png"><img class="alignnone size-large wp-image-2575" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.08-1024x797.png" alt="Screenshot 2014-10-20 19.49.08" width="584" height="454" /></a>`

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">PFUser *jill; // username "jill"
[acl setReadAccess:true forUser:jill];</pre>

Jack and Jill can see the post now, and Jack is the only person who can edit it. But maybe you want to be able to make sure the content of Jack's post is appropriate, and so you want a group of administrators to be able to see the post, and to delete it if something is wrong with it. Once you've assigned these users to a [Role](https://parse.com/docs/ios_guide#roles/iOS) named `admin`, you could add permissions on that role to the ACL. Given that delete is included in “write” permissions, you would check both read and write in the ACL editor.

`<a href="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.15.png"><img class="alignnone size-large wp-image-2576" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.15-1024x802.png" alt="Screenshot 2014-10-20 19.49.15" width="584" height="457" /></a>`

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[acl setReadAccess:true forRoleWithName:@"admin"];
[acl setWriteAccess:true forRoleWithName:@"admin"];</pre>

What if Jack wants to publish his post publicly? Now you want to give the post public read permissions. Simply check the read checkbox in the public permissions row at the top of the editor, and anyone can now see the post.

`<a href="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.18.png"><img class="alignnone size-large wp-image-2577" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.49.18-1024x795.png" alt="Screenshot 2014-10-20 19.49.18" width="584" height="453" /></a>`

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">[acl setPublicReadAccess:true];</pre>

Notice that all of the users and roles in the ACL now have read permissions checked automatically. That is because you have enabled public read, so anybody can read Jack's post. But public write is still disabled, so the only users who can edit the post are Jack and all users that belong to the `admin` role. Click “Save ACL” and now your post object will have all of these permissions set, summarized in its cell by “Public Read, 1 Role, 2 Users”.

`<a href="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.53.15.png"><img class="alignnone size-large wp-image-2578" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-19.53.15-1024x184.png" alt="Screenshot 2014-10-20 19.53.15" width="584" height="104" /></a>`

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">post.ACL = acl;
[post saveInBackground];</pre>

An undefined ACL acts the same as an ACL with public read and write permissions enabled, so when you first click on a cell without an ACL defined, the editor will show the public read and write boxes checked. To add private permissions on a role or user, you must first uncheck public read and/or write.

[<img class="alignnone size-large wp-image-2579" src="{{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-15.02.30-1024x797.png" alt="Screenshot 2014-10-20 15.02.30" width="584" height="454" />]({{ site.url }}/assets/wp-content/uploads/2014/10/Screenshot-2014-10-20-15.02.30.png)

We are excited to give Parse users a fresh, simple way to add and manage ACLs in the data browser with the new ACL editor. [Go try it out](https://www.parse.com/apps/), and as always, [let us know what you think](https://www.parse.com/help)!