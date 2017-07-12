---
id: 2413
title: 'Parse Security III - Are You On the List?'
date: 2014-07-14T17:44:25+00:00
author: Bryan Klimt
layout: post
guid: http://blog.parse.com/?p=2413
permalink: /learn/engineering/parse-security-iii-are-you-on-the-list/
dsq_thread_id:
  - "3687646299"
categories:
  - Engineering
---
In <a href="http://blog.parse.com/2014/07/07/parse-security-ii-class-hysteria/" target="_blank">Part II</a>, we looked at class-level permissions, which allow you to quickly set permissions for an entire class at once. But often, the different objects in a class will need to be accessible by different people. For example, a user's private personal data should be accessible only to them. For that, you have to use an Access Control List, usually referred to as an ACL. If you have a production app and you aren't using ACLs, you're almost certainly doing it wrong.

An ACL specifies a set of users who can read or write an object's data. So, before you can use ACLs, you have to have Users. There are many ways to handle users on Parse. You can have usernames and passwords, or you can use <a href="https://developers.facebook.com/docs/facebook-login/v2.0" target="_blank">Facebook Login</a>. If you don't want to make your users create usernames just to log in, you can even use Parse's <a href="http://blog.parse.com/2012/04/02/protect-user-data-with-new-parse-features/" target="_blank">automatic anonymous users feature</a>. That allows you to create a user and log them in on a particular device in a secure way. If they later decide to set a username and password, or link the account with Facebook, then they can then log in from any other device. Setting up automatic anonymous users is easy, so you have no excuse not to protect per-user data!

<pre class="brush: objc; gutter: false">[PFUser enableAutomaticUser];</pre>

Once you have a user, you can start using ACLs. To set an ACL on the current user's data to not be publicly readable, all you have to do is:

<pre class="brush: objc; gutter: false">PFUser *user = [PFUser currentUser];
user.ACL = [PFACL ACLWithUser:user];</pre>

Most apps should do this. If you store any sensitive user data, such as email addresses or phone numbers, you need to set an ACL like this so that the user's private information isn't visible to other users. If an object doesn't have an ACL, it's readable and writeable by everyone. The only exception is the `_User` class. We never allow users to write each other's data, but they can read it by default. (If you as the developer need to update other `_User` objects, remember that your master key can provide the power to do this.) To make it super easy to create user-private ACLs for every object, we have a way to set a default ACL that will be used for every new object you create.

<pre class="brush: objc; gutter: false">[PFACL setDefaultACL:[PFACL ACL] withAccessForCurrentUser:YES];</pre>

If you want the user to have some data that is public and some that is private, it's best to have two separate objects. You can add a pointer to the private data from the public one.

<pre class="brush: objc; gutter: false">PFObject *privateData = [PFObject objectWithClassName:@"PrivateUserData"];
privateData.ACL = [PFACL ACLWithUser:[PFUser currentUser]];
[privateData setObject:@"555-5309" forKey:@"phoneNumber"];

[PFUser setObject:privateData forKey:@"privateData"];</pre>

Of course, you can set different read and write permissions on an object. For example, this is how you would create an ACL for a public post by a user, where anyone can read it:

<pre class="brush: objc; gutter: false">PFACL *acl = [PFACL ACL];
[acl setPublicReadAccess:true];
[acl setWriteAccess:true forUser:[PFUser currentUser]];</pre>

Sometimes, it's inconvenient to manage permissions on a per-user basis, and you want to have groups of users who get treated the same. For example, your app may have a group of admins who are the only ones who can write data. Roles are are a special kind of object that let you create a group of users that can all be assigned to the ACL. The best thing about roles is that you can add and remove users from a role without having to update every single object that is restricted to that role. To create an object that is writeable only by admins:

<pre class="brush: objc; gutter: false">PFACL *acl = [PFACL ACL];
[acl setPublicReadAccess:true];
[acl setWriteAccess:true forRoleWithName:@"admins"];</pre>

Of course, this snippet assumes you've already created a role called “admins”. In many cases, this is reasonable, because you have a small set of special roles you can set up while developing your app. But sometimes you will need to create roles on the fly. In a future blog post, we'll look into how you can use ACLs and roles to manage a user's data so that it will only be visible to their friends.

So far in this security series, we've covered class-level permissions and ACLs. These features are sufficient to secure the vast majority of apps. In Part IV, we'll look at how you can use Cloud Code to secure apps even in rare edge cases.

<span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/06/30/parse-security-i-are-you-the-key-master/" target="_blank">Part I</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/07/07/parse-security-ii-class-hysteria/" target="_blank">Part II</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/07/21/parse-security-iv-ahead-in-the-cloud/" target="_blank">Part IV</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parse.com/2014/07/28/parse-security-v-how-to-make-friends/" target="_blank">Part V</a></span>