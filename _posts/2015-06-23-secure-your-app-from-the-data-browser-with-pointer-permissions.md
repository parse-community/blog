---
id: 3617
title: Secure Your App from the Data Browser with Pointer Permissions
date: 2015-06-23T13:27:13+00:00
author: drewgross
layout: post
guid: http://blog.parse.com/?p=3617
permalink: /learn/secure-your-app-from-the-data-browser-with-pointer-permissions/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3873015178"
categories:
  - Engineering
  - Learn
tags:
  - CLPs
  - databrowser
  - permissions
  - security
---
Parse tools are great for building an apps quickly, but when your first priority is shipping your app soon, it's easy to forget important steps like setting ACLs on every object. If you skip this, you'll face the time-consuming and error-prone task of going back and plugging all your security holes; you may even put off securing your app until it's too late. At Parse, we think moving fast should never mean ignoring security. That's why we're launching a new kind of Class Level Permission, called [pointer permissions](https://parse.com/docs/ios/guide#security-pointer-permissions), that makes it even easier to secure your app quickly without writing any new cloud code or client code.

Pointer permissions let you secure all of the objects in a class of your Parse app at once, directly from the [data browser](https://www.parse.com/apps/). Oftentimes the user that should have permissions for an object is already stored in that object, in a [pointer](https://parse.com/docs/ios/guide#relations-using-pointers) field like `owner` or `creator`. So rather than saving the `owner` of an object both as a pointer and in an ACL every time you create an object, now you can just add a pointer permission to the `owner` field in the data browser, and then all objects in the collection will only be accessible by the user listed as the owner.

Let's go through an example. Say you have a `Message` collection, and each message has a `sender` and a `receiver`, which are each pointers to a `User`. You want to make sure only the sender and receiver of the message can see the message.<figure id="attachment_3618" style="width: 938px" class="wp-caption alignnone">

<img class="wp-image-3618 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-1.png" alt="Pointer Permissions 1" width="938" height="189" srcset="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-1.png 938w, {{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-1-300x60.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-1-875x176.png 875w" sizes="(max-width: 938px) 100vw, 938px" /><figcaption class="wp-caption-text">Note the sender and receiver fields are both pointers.</figcaption></figure> 

Open the Security editor in the data browser for the `Message` class, and set pointer permissions to allow Read for both of those pointer fields. Make sure that the public Read and Write checkboxes are disabled, type the pointer field names, and click the Read checkbox for each. If `Message` has existing ACLs on it, you'll also need to make sure your new pointer permissions are compatible with the existing ACLs.

<img class="alignnone size-full wp-image-3619" src="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-2.png" alt="Pointer Permissions 2" width="660" height="529" srcset="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-2.png 660w, {{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-2-300x240.png 300w" sizes="(max-width: 660px) 100vw, 660px" />

That's it! Now for every object in the `Message` class, only the users stored in the `sender` and `receiver` fields can get or find that message object.

Now maybe you want to allow the sender of the message to edit the message, but make sure that the receiver can only read it. Just click the Write checkbox for the `sender` pointer, save the CLP, and you're done!

<img class="alignnone size-full wp-image-3620" src="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-3.png" alt="Pointer Permissions 3" width="660" height="529" srcset="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-3.png 660w, {{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-3-300x240.png 300w" sizes="(max-width: 660px) 100vw, 660px" />

Now every object in the Message class will only be readable by the `sender` and `receiver`, and only writable by the `sender`.

Note that, just like other Class Level Permissions, pointer permissions interact with ACLs in a way that you may not expect. [As documented in our security guide](https://parse.com/docs/data#security-interaction), CLPs and ACLs are both needed to allow a request in order for that request to work. Because pointer permission are a part of CLPs, this means that in order for an object to be accessible, your pointer permission, ACLs, and other CLPs must all grant access. For example, if you have an object with an ACL that gives read permission to only the user with ID `8WsJzurgHY` and a read pointer permission set for the `sender`, whose ID is `zSLiNbCcl2R`, then neither user will be allowed by both the pointer permission and the ACL, and no user will be able to read the object.

<img class="alignnone wp-image-3621 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-4.png" alt="Pointer Permissions 4" width="755" height="51" srcset="{{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-4.png 755w, {{ site.url }}/assets/wp-content/uploads/2015/06/Pointer-Permissions-4-300x20.png 300w" sizes="(max-width: 755px) 100vw, 755px" />

Because of the way CLPs and ACLs interact, pointer permissions are best used on classes where the ACLs haven't been touched yet, so there are no unintended contradictions. Public ACLs are the default state, so if you haven't thought about security or ACLs, pointer permissions may be a good first step to securing your app since they can be used without worrying about the ACLs. If you later want to add more complex security functionality, such as removing write permission from the `sender` of a message after the `receiver` has read it, you can start using ACLs or cloud code at any time, and easily remove the pointer permission whenever you're ready.

We're excited to give Parse developers this powerful new feature, and hope that it helps you launch more secure apps. Go check it out in the Security editor in the [data browser](https://www.parse.com/apps/), and as always, [let us know what you think](https://www.parse.com/help)!