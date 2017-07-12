---
id: 2689
title: Secure Your App, One Class at a Time
date: 2015-02-23T21:29:17+00:00
author: JamieKarraker
layout: post
guid: http://blog.parse.com/?p=2689
permalink: /learn/secure-your-app-one-class-at-a-time/
post_format:
  - basic
dsq_thread_id:
  - "3607554758"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Learn
tags:
  - mustread
  - security
---
Security is one of the most critical aspects of releasing a production app, and with Parse there are lots of ways you can secure your app. As Bryan laid out in his [5-part security series](http://blog.parse.com/2014/06/30/parse-security-i-are-you-the-key-master/), Parse provides a powerful security toolset, including the [master key](http://blog.parse.com/2014/06/30/parse-security-i-are-you-the-key-master/), [Access Control Lists (ACLs)](https://parse.com/docs/data#security-objects), [Class-Level Permissions (CLPs)](https://parse.com/docs/data#security-classes), and [Cloud Code](http://blog.parse.com/2014/07/21/parse-security-iv-ahead-in-the-cloud/). Every successful production app uses some features out of this toolset, and we are always working to improve its ease of use for Parse developers to take advantage of these effective tools. A few months ago, we released an [updated ACL editor](http://blog.parse.com/2014/10/21/get-off-my-lawn-a-new-way-to-control-access-to-your-app/), making it much easier to view and edit ACLs in the data browser. Today we are releasing a new and improved CLP editor, so you can view and edit your app's Class-Level Permissions just as easily as its object-level ACLs!

Just like ACLs, CLPs are lists of [users](https://parse.com/docs/ios_guide#users/iOS) or [roles](https://parse.com/docs/ios_guide#roles/iOS) that can access a specific class of your app. As with ACLs, you can give each user/role read or write access. Let's see how this works in an example. Say we have a restaurant app, with a `Restaurant` class. To edit the CLPs for the `Restaurant` class, click the 'Security' button at the top of the data browser. This will open the new CLP editor, and you will notice that the default permissions for every class is public read and write ([with some exceptions](https://parse.com/docs/data#security-edgecases)).

[<img class="alignnone size-large wp-image-2690" src="{{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.35.50-1024x812.png" alt="Screenshot 2015-02-10 20.35.50" width="584" height="463" />]({{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.35.50.png)

You may notice that this looks strikingly familiar to the ACL editor. Right now the `Restaurant`s are completely public, meaning that any user of the app can see every `Restaurant`, and can change every `Restaurant`. We probably don't want `Restaurant`s to be completely public, because then any user could just delete all the `Restaurant`s in the database and our app wouldn't do much after that. So, let's disable public writes.

[<img class="alignnone size-large wp-image-2691" src="{{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.42.18-1024x799.png" alt="Screenshot 2015-02-10 20.42.18" width="584" height="456" />]({{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.42.18.png)

Now any user can see all the `Restaurant`s, but nobody can change any `Restaurant`s (without the master key). But maybe I want my own user to be able to create new `Restaurant`s, or update old ones. I can add my user to the CLP editor by typing in my username or objectId, and allow writes for that user by clicking the “Write” checkbox.

[<img class="alignnone size-large wp-image-2692" src="{{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.48.07-1024x805.png" alt="Screenshot 2015-02-10 20.48.07" width="584" height="459" />]({{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.48.07.png)

Now the `jamie` user can do whatever he wants with all of the objects in the `Restaurant` class, while no one else can write to this class. But maybe there are some other users that should be able to write to this class, and I don't want to keep adding each user one-by-one. Let's use a [role](https://parse.com/docs/ios_guide#roles/iOS), which we've named `admin` . First we add all the users we want to have write access to the `admin` role, then we type the role name into the next row, and finally we enable write access by clicking on the “Write” checkbox.

[<img class="alignnone size-large wp-image-2693" src="{{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.59.40-1024x804.png" alt="Screenshot 2015-02-10 20.59.40" width="584" height="459" />]({{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-10-20.59.40.png)

Any user in the `admin` role can now make changes to the `Restaurant` class. But what if we want to let some other group of users - those in the `chef` role - add new `Restaurant`s, but not be able to edit existing `Restaurant`s? To do that, we first click the settings gear at the top-right of the editor, and switch to the “Advanced” mode of the editor. In the advanced mode we can set more specific permissions on the class for different request types.

[<img class="alignnone size-large wp-image-2695" src="{{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-11-16.28.18-1024x529.png" alt="Screenshot 2015-02-11 16.28.18" width="584" height="302" />]({{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-11-16.28.18.png)

There are 6 types of permissions you can manage in the CLP editor, based on the different types of requests you can make to Parse. We group these permissions into “Read” and “Write” categories, with the exception of “Add Field”.

<ul class="standard-list">
  <li>
    <strong>Read</strong>: <ul class="standard-list">
      <li>
        <strong>Get</strong> — With Get permissions enabled, users can fetch objects in this class if they know their objectIds.
      </li>
      <li>
        <strong>Find</strong> — Anyone with Find permissions enabled can query all of the objects in the class, even if they don’t know their objectIds. Any table with public Find permissions will be completely readable by the public, unless you put an ACL on each object. (<a href="https://parse.com/docs/data#security-edgecases">with some exceptions</a>)
      </li>
    </ul>
  </li>
  
  <li>
    <strong>Write</strong>: <ul class="standard-list">
      <li>
        <strong>Update</strong> — Anyone with Update permissions enabled can modify the fields of any object in the class. For publicly readable data, such as game levels or assets, you should disable this permission.
      </li>
      <li>
        <strong>Create</strong> — Like Update, anyone with Create permissions enabled can create new objects of a class. As with the Update permission, you'll probably want to turn this off for publicly readable data.
      </li>
      <li>
        <strong>Delete</strong> — With this permission enabled, users can delete any object in the class. All they need is its objectId.
      </li>
    </ul>
  </li>
  
  <li>
    <strong>Add Field</strong> — Parse classes have schemas that are inferred when objects are created. While you're developing your app, this is great, because you can add a new field to your object without having to make any changes on the backend. But once you ship your app, it's very rare to need to add new fields to your classes automatically. So you should just about always turn off this permission for all of your classes when you submit your app to the public.
  </li>
</ul>

Going back to our example, we wanted to ensure that all users in the `chef` role could create `Restaurant`s, but we don't want them to be able to update or delete any existing restaurants. To accomplish this we simply add a row for the `chef` role, and check “Create".

<img class="alignnone size-large wp-image-2694" src="{{ site.url }}/assets/wp-content/uploads/2015/02/Screenshot-2015-02-11-16.27.43-1024x526.png" alt="Screenshot 2015-02-11 16.27.43" width="584" height="300" />

We could add permissions for any of the other types of requests in just the same way. CLPs are an extremely powerful tool for quickly and easily securing your app. But before you add a bunch of CLPs to your app, make sure you understand [how CLPs and ACLs interact with each other](https://parse.com/docs/data#security-interaction).

We are excited to continue giving Parse developers better ways to secure their apps, and we hope this helps you launch safer, more secure production apps! Go check out the new editor in the [data browser](https://www.parse.com/apps/), and as always, [let us know what you think](https://www.parse.com/help)!