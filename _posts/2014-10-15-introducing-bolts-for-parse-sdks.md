---
id: 2558
title: Introducing Bolts for Parse SDKs
date: 2014-10-15T16:39:28+00:00
author: Nikita Lutsenko
layout: post
guid: http://blog.parse.com/?p=2558
permalink: /learn/introducing-bolts-for-parse-sdks/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3607570644"
image: /wp-content/uploads/2014/10/bolts1.jpg
categories:
  - Learn
---
Earlier this year we introduced the <a title="Bolts Framework" href="http://github.com/boltsframework" target="_blank">Bolts Framework</a>, a set of low-level libraries to make developing mobile apps easier and faster. One of the core components of Bolts is **Tasks**. Tasks brings the programming model of <a title="JavaScript Promises" href="http://blog.parse.com/2013/01/29/whats-so-great-about-javascript-promises/" target="_blank">JavaScript Promises</a> to iOS and Android.

Today, we are proud to open an entirely new set of public APIs with the new versions of our <a title="Parse iOS SDK" href="https://parse.com/docs/downloads/" target="_blank">iOS</a>, <a title="Parse OS X SDK" href="https://parse.com/docs/downloads/" target="_blank">OS X</a> and <a title="Parse Android SDK" href="https://parse.com/docs/downloads/" target="_blank">Android</a> SDKs. Inside this release you will find that every method that was once intended to be asynchronous now has a Tasks-based counterpart. Organizing complex asynchronous code is now much more manageable in addition to being easier and faster to implement.

For example, here's how you can find posts, mark them all as published, and update the UI on iOS:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">PFQuery *query = [PFQuery queryWithClassName:@"Post"];
[query whereKey:@"published" notEqualTo:@YES];
[[query findObjectsInBackground] continueWithSuccessBlock:^id(BFTask *task) {
  NSArray *results = task.result;

  NSMutableArray *saveTasks = [NSMutableArray arrayWithCapacity:[results count]];
  for (PFObject *post in results) {
    post[@"published"] = @YES;
    [saveTasks addObject:[post saveInBackground]];
  }
  return [BFTask taskForCompletionOfAllTasks:saveTasks];
}] continueWithExecutor:[BFExecutor mainThreadExecutor] withSuccessBlock:^id(BFTask *task) {
  _postStatusLabel.text = @"All Posts Published!";
  return task;
}];</pre>

And here's how it looks on Android:

<pre class="EnlighterJSRAW" data-enlighter-language="java">ParseQuery&lt;ParseObject&gt; query = ParseQuery.getQuery("Post");
query.whereNotEqualTo("published", true);
query.findInBackground().onSuccessTask(new Continuation&lt;List&lt;ParseObject&gt;, Task&lt;Void&gt;&gt;() {
  @Override
  public Task&lt;Void&gt; then(Task&lt;List&lt;ParseObject&gt;&gt; task) throws Exception {
    List&lt;ParseObject&gt; results = task.getResult();
        
    List&lt;Task&lt;Void&gt;&gt; saveTasks = new ArrayList&lt;Task&lt;Void&gt;&gt;();
    for (final ParseObject post : results) {
      post.put("published", true);
      saveTasks.add(post.saveInBackground());
    }
    return Task.whenAll(saveTasks);
  }
}).onSuccess(new Continuation&lt;Void, Void&gt;() {
  @Override
  public Void then(final Task&lt;Void&gt; task) {
    mPostStatusTextView.setText(String.format("All Posts Published!"));
    return null;
  }
}, Task.UI_THREAD_EXECUTOR);</pre>

As you can see, there's a lot of advantages to this approach when it comes to branching, chaining tasks, parallelizing across multiple threads, and handling complex errors.

We are really excited about this release and would love to hear your feedback. Let us know what you think!