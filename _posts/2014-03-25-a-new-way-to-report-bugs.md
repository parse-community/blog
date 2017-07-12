---
id: 2146
title: A New Way to Report Bugs
date: 2014-03-25T17:27:19+00:00
author: Héctor Ramos
layout: post
guid: http://blog.parse.com/?p=2146
permalink: /announcements/a-new-way-to-report-bugs/
dsq_thread_id:
  - "3689856175"
categories:
  - Announcements
  - Engineering
---
If [the way to a developer's heart is great documentation](http://blog.parse.com/2012/01/11/designing-great-api-docs/), the way to stay there is through great support. Here at Parse we've always been happy to help through our community forums as well as through face to face conversations at one of our many developer meetups. As our community has grown, it has become increasingly important to provide a way to surface sensitive issues in a timely manner. Today, I'd like to introduce a new addition to our Help portal, the _Report a Bug_ button:

<center>
  <img class="aligncenter size-full wp-image-2190" alt="reportbug" src="{{ site.url }}/assets/wp-content/uploads/2014/03/reportbug.png" width="388" height="60" />
</center>This will allow you to create a new bug report which can be associated with one of your Parse apps. You will also be able to tag your bug report with one of the top level product categories. This will help us triage bug reports as well as help other Parse developers discover related issues.

Let's take for example a simple messaging app that allows people to send photos to their friends. These are stored in an `Attachment`s class and I have set up an array of pointers through a `Pointer` field, `attachments`, in my `Message` class. Imagine for a second that the query I am using is suddenly not working and I've decided to submit a bug report. This sounds like a great fit for the `iOS/OS X SDK > Queries` category.

<center>
  <img class="aligncenter size-full wp-image-2201" alt="bugtoolreportflow" src="{{ site.url }}/assets/wp-content/uploads/2014/03/bugtoolreportflow.png" width="900" height="260" />
</center>The above is a great example of a simple but concise bug report. It describes the unexpected behavior and provides all the code needed for our engineers to reproduce the issue. Once submitted, the report will be assigned to one of our engineers, and you will receive notifications on status updates for this specific report, such as “Fixed” or “Needs More Info”.

<center>
  <img class="aligncenter size-full wp-image-2206" alt="bugtoolstatus" src="{{ site.url }}/assets/wp-content/uploads/2014/03/bugtoolstatus.png" width="943" height="398" />
</center>You can also search for existing bug reports and known issues through the 

[Bug Tool](https://developers.facebook.com/x/bugs/trending/). You may subscribe to existing bug reports should you find that you’re running into a similar issue.

<center>
  <img class="aligncenter size-full wp-image-2207" alt="subscribe" src="{{ site.url }}/assets/wp-content/uploads/2014/03/subscribe.png" width="313" height="249" />
</center>I'm really excited about the launch of this new support channel, which will allow us to provide faster turnaround times for your bug reports. As always, feel free to leave any feedback on our 

[community forums](https://parse.com/help)!