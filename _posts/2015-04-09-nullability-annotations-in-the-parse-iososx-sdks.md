---
id: 3405
title: Nullability Annotations in the Parse iOS/OSX SDKs
date: 2015-04-09T18:38:16+00:00
author: nikitalutsenko
layout: post
guid: http://blog.parseplatform.org/?p=3405
permalink: /learn/nullability-annotations-in-the-parse-iososx-sdks/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3687480001"
categories:
  - Engineering
  - Learn
tags:
  - iOS
  - nullability
  - objects
  - swift
---
One of the many benefits of the Swift language is that it transparently interoperates with Objective-C code. But previously, if you were using existing frameworks like a Parse SDK or Objective-C code in your app, there were some missing pieces.

In Swift there's a strong distinction between optional and non-optional references, for example `PFObject` versus `PFObject?`. However, Objective-C represents both of these types as a pointer (`PFObject *`). When simple code like that is used in Swift, it will be converted into an implicitly unwrapped optional, `PFObject!`.

With the recent release of Xcode 6.3, there appears to be a major improvement for this issue: nullability annotations in Objective-C. Here at Parse, we jumped straight on it. In the last couple of Parse SDK releases, you might have noticed that most of our types are now annotated with `PF_NULLABLE` and `PF_NONNULL`. These annotations not only majorly expand the Swift capabilities of our SDKs, but also help the compiler to understand your code better. As a result, you can now optimize even further, regardless of the language you're using.

Previously, it was a requirement to test your code thoroughly, as you weren't warned about anything that might go wrong:

<pre class="line-numbers"><code class="language-swift">class PFObject {
  func setObject(object: AnyObject!, forKey key: String!)
}

func createObjectWithName(name: String, forKey key: String?) {
  let object = PFObject(className: "Object")
  object.setObject(name, forKey: key)
  // The application will crash here as 'key' cannot be nil.
}</code></pre>

Now, with Nullability Annotations, your Swift code will behave slightly differently with Xcode 6.3:

<pre class="line-numbers"><code class="language-swift">class PFObject {
  func setObject(object: AnyObject, forKey key: String)
}

func createObjectWithName(name: String, forKey key: String?) {
  let object = PFObject(className: "Object")
  object.setObject(name, forKey: key) 
  // You will get a compiler error, saying that key can't be `nil`.
}</code></pre>

Your Objective-C code also gets an improvement:

<pre class="line-numbers"><code class="language-objectivec">@interface PFObject : NSObject

- (void)setObject:(PF_NONNULL_S id)object 
           forKey:(PF_NONNULL NSString *)key;

@end

- (void)createObjectWithName:(NSString *)name 
                      forKey:(nullable NSString *)key {
  PFObject *object = [PFObject objectWithClassName:@"Object"];
  [object setObject:name forKey:key]; 
  // You will get a compiler warning, saying that key can't be 'nil'.
}</code></pre>

From our experience working with these annotations inside the SDK, it's been tremendously helpful for us.

Nullability annotations are already available in the latest <a title="iOS/OSX SDKs" href="https://parse.com/docs/downloads/" target="_blank">iOS/OSX SDKs</a>. We're excited to see how this might improve your development workflow.

<a title="Let us know what you think!" href="https://groups.google.com/group/parse-developers" target="_blank">Let us know what you think!</a>