---
id: 2342
title: Building Apps with Parse and Swift
date: 2014-06-06T05:18:21+00:00
author: Fosco Marotto
layout: post
guid: http://blog.parse.com/?p=2342
permalink: /announcements/building-apps-with-parse-and-swift/
dsq_thread_id:
  - "3685461122"
categories:
  - Announcements
  - Engineering
---
On Monday at WWDC 2014, Apple released a new programming language called <a href="https://developer.apple.com/swift/" target="_blank">Swift</a>. <span class="_5yl5" data-reactid=".2a.$mid=11402071848955=25bc5de3abb1125ea57.2:0.0.0.0.0"><span class="null">As always, when we see that developers are excited about a new language or platform, we work quickly to make sure Parse can support that momentum.</span></span> We're excited about Swift because it brings a whole host of new language features to iOS and OS X development. Swift's type inference will save developers a ton of typing and generics will reduce runtime errors by giving us strongly-typed collections. To learn more about Swift, checkout Apple's <a href="https://itunes.apple.com/us/book/the-swift-programming-language/id881256329?mt=11" target="_blank">reference book</a>.

One of the best things about Swift for existing iOS developers is that it's fully compatible with existing Objective-C libraries, including system libraries like Cocoa and third-party libraries like Parse. To start using Parse in your Swift projects:

* Add a new file to your project, an Objective-C .m file.
  
* When prompted about creating a bridge header file, approve the request.
  
* Remove the unused .m file you added.
  
* Add your Objective-C import statements to the created bridge header .h file:

<pre class="brush: c; gutter: false">#import &lt;Parse/Parse.h&gt;
// or #import &lt;ParseOSX/ParseOSX.h&gt;
</pre>

<a href="http://stackoverflow.com/questions/24002369/how-to-call-objective-c-code-from-swift/24005242#24005242" target="_blank">This StackOverflow answer</a> gives a more thorough explanation.

Once you've added Parse to your bridge header, you can start using the Parse framework in your Swift project. To help you get started, we've added Swift example code to our entire [iOS/OSX documentation](https://parse.com/docs/ios_guide). For example, this is all you need to do to save an object to Parse:

<pre class="brush: c; gutter: false">var gameScore = PFObject(className: "GameScore")
gameScore.setObject(1337, forKey: "score")
gameScore.setObject("Sean Plott", forKey: "playerName")
gameScore.saveInBackgroundWithBlock { 
    (success: Bool!, error: NSError!) -&gt; Void in
    if success {
        NSLog("Object created with id: (gameScore.objectId)")
    } else {
        NSLog("%@", error)
    }
}
</pre>

To then query for the object by its id:

<pre class="brush: c; gutter: false">var query = PFQuery(className: "GameScore")
query.getObjectInBackgroundWithId(gameScore.objectId) {
    (scoreAgain: PFObject!, error: NSError!) -&gt; Void in
    if !error {
        NSLog("%@", scoreAgain.objectForKey("playerName") as NSString)
    } else {
        NSLog("%@", error)
    }
}
</pre>

That's everything you need to know to start using <a href="https://itunes.apple.com/us/book/the-swift-programming-language/id881256329?mt=11" target="_blank">Swift</a> with Parse. For more examples, don't forget to visit our [iOS/OSX documentation](https://parse.com/docs/ios_guide). We can't wait to see what you build with it!