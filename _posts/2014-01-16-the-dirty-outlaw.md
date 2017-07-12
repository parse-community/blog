---
id: 1945
title: The Dirty Outlaw
date: 2014-01-16T18:03:40+00:00
author: listiarsowastuargo
layout: post
guid: http://blog.parse.com/?p=1945
permalink: /learn/engineering/the-dirty-outlaw/
dsq_thread_id:
  - "3688802007"
categories:
  - Engineering
---
For every platform, Parse SDKs make it awesomely flexible to save a `ParseObject`. Sometimes we want to know whether a particular `ParseObject` has been updated recently but not saved yet. For example, one use case would be if we are making a word processing app. The user opens a document, writes some poems and is about to quit the application. The app needs to prompt the user to save the document. For this, the app needs to know that the document has been edited but not yet saved to the server.

One way to do this is by checking the previous value of the document every time we want to quit. Let's say we have a document object. Every time the user saves the document, we'd have to update a copy of the previous value of the text, and we'd have to check whether the value changed every time the user quits the app.

<pre class="brush: java gutter: false">ParseObject document = new ParseObject("document");
String previousText = "";

public void saveDocument() {
  document.saveInBackground();
  previousText = document.get("text");
}

public void quit() {
  if (!previousText.equals(document.get("text")) {
    saveDialog.show();
  }
}</pre>

What if we want to do the "dirty" check not only on quit but also on share, on like, on seen, and on <put random verbs here>? We could factor that check out into a separate method. What if we want to check the dirtiness of not only the text of the document, but also the images? What if in the app we want to check the dirtiness of not only the document, but also the toolbar and bookmarks? Pretty soon it adds up.

Here at Parse, we encourage you to be lazy so you can focus on doing more important things. For this particular document's problem, we have `isDirty` to the rescue! Now, all you have to do is:

<pre class="brush: java gutter: false">public void quit() {
  if (document.isDirty()) {
    saveDialog.show();
  }
}</pre>

Or for more refined requirements:

<pre class="brush: java gutter: false">public void quit() {
  if (document.isDirty("text")) {
    saveDialog.show("Document is not saved yet. Save now?");
  } else if (document.isDirty("images")) {
    saveDialog.show("One of your images is not saved yet. Save now?");
  } else if (bookmarks.isDirty()) {
    saveDialog.show("You have updated your bookmarks. Save your bookmarks?");
  }
}</pre>

These APIs are available across all of our SDKs, so go check them out today!