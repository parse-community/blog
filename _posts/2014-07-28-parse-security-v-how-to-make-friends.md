---
id: 2415
title: 'Parse Security V - How to Make Friends'
date: 2014-07-28T17:28:54+00:00
author: bryanklimt
layout: post
guid: http://blog.parseplatform.org/?p=2415
permalink: /learn/engineering/parse-security-v-how-to-make-friends/
dsq_thread_id:
  - "3687697544"
categories:
  - Engineering
---
In the first four parts of this five-part series on how to secure your Parse app, we've taken a look at all of the different features that Parse has to help you secure your app and your users' data. In Part V, let's put it all together and take a look at a real example of how you can use these features to solve a complex use case.

Most classes in your app will fall into one of a couple of easy-to-secure categories. For fully public data, you can use class-level permissions to lock down the table to put publicly readable and writeable by no one. For fully private data, you can use ACLs to make sure that only the user who owns the data can read it. But occasionally, you'll run into situations where you don't want data that's fully public or fully private. For example, you may have a social app, where you have data for a user that should be readable only to friends whom they've approved. For this you'll need to use ACLs, roles, and Cloud Code together to enable exactly the sharing rules you desire.

If you aren't clear on those features, go back and read Parts II - IV of this series. Once you're clear on what ACLs, roles, and Cloud Code are, let's dig into some code. The first thing you'll need to do is set up a place for the user's social data to live. Let's assume you want to create a “FriendData” object for each user that stores the data that should be visible to their friends. To create this object for each new user, you can use an `afterSave` handler.

<pre class="brush: javascript; gutter: true">Parse.Cloud.afterSave(Parse.User, function(request, response) {
    var user = request.object;
    if (user.existed()) { return; }
    var roleName = "friendsOf_" + user.id;
    var friendRole = new Parse.Role(roleName, new Parse.ACL(user));
    return friendRole.save().then(function(friendRole) {
        var acl = new Parse.ACL();
        acl.setReadAccess(friendRole, true);
        acl.setReadAccess(user, true);
        acl.setWriteAccess(user, true);
        var friendData = new Parse.Object("FriendData", {
          user: user,
          ACL: acl,
          profile: "my friend profile"
        });
        return friendData.save();
    });
});</pre>

The first couple of lines just set up the request. On line 3, we check to see if the user already existed. If so, then we've already completed this setup, and there's no reason to do it again. On lines 4-6, we create a new role. This role will represent the friends of the user being created. We generate a unique name for the role. The role is created with an ACL that only allows this user to read or write it. In other words, only the user themselves can decide who their friends are.

Once the role is created, lines 7-10 create an ACL that grants read permission to friends of the user. And of course the user receives read and write permission for their own social data. Once the ACL is set up, lines 11-16 actually create the object. The user is set on the object so that it can be found with a query later. The `profile` field is set as an example of data that will be readable only to the users friends.

So, that's everything you need to set up an object for every user that will be readable only to people in their special friends role. Of course, that's worthless without any way for people to get added to your friends list, so we need to address that with another function. Technically, this function could be written in any client and be secure, but for the sake of consistency, let's make another Cloud Code function. This function will be called “friend."  It will take one parameter: the objectId of the person to friend. This person will then be added to the friends role of the current user.

<pre class="brush: javascript; gutter: true">Parse.Cloud.define("friend", function(request, response) {
    var userToFriend = new Parse.User();
    userToFriend.id = request.params.friendId;

    var roleName = "friendsOf_" + request.user.id;
    var roleQuery = new Parse.Query("_Role");
    roleQuery.equalTo("name", roleName);
    roleQuery.first().then(function(role) {
        role.getUsers().add(userToFriend);
        return role.save();

    }).then(function() {
        response.success("Success!");    
    });
});</pre>

Lines 1-3 just set up the function and create a User object to represent the person being added to the friend role. Lines 5-8 fetch the Role object using its name. A Role is just a special kind of Parse object, so we need to fetch it before we can modify it. Once the role has been fetched, we add this user to it on line 9, and save it on line 10. That's all there is to it! Now we have an easy way to store data that's only accessible to a user's friends.

Over the course of this five-part series on how to secure your Parse app, we've looked at a lot of features. You know the about the various keys used to access your app. We've looked at the permissions you can set to lock down a whole class. You've learned about ACLs and how they can secure per-user data. And we've even dived deep into a particular example of how you can use Cloud Code to tackle even the trickiest data models.

We hope that you'll use these tools to do everything you can to keep your app's data and your users' data secure. Together, we can make the web a safer place.

<span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/06/30/parse-security-i-are-you-the-key-master/" target="_blank">Part I</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/07/parse-security-ii-class-hysteria/" target="_blank">Part II</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/14/parse-security-iii-are-you-on-the-list/" target="_blank">Part III</a></span>   <span style="text-decoration: underline;"><a href="http://blog.parseplatform.org/2014/07/21/parse-security-iv-ahead-in-the-cloud/" target="_blank">Part IV</a></span>