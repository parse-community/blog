---
id: 3886
title: The New Parse Developer Experience
date: 2015-12-09T13:16:44+00:00
author: Andrew Imm
layout: post
guid: http://blog.parse.com/?p=3886
permalink: /announcements/the-new-parse-developer-experience/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - small
dsq_thread_id:
  - "4390238629"
image: /wp-content/uploads/2015/12/02-Dashboard-Writeup-Illustration-Final-ish.png
categories:
  - Announcements
tags:
  - dashboard
  - reactjs
  - webpack
---
Today we're excited to give our users beta access **to the new Parse Dashboard.** It's the result of months of effort spent working to bring you the best developer experience we can. It's more than just a fresh coat of paint though — it's completely rebuilt from the ground up with tools like React and Webpack to make your workflow faster and more seamless.

This release is also huge for us, the developers behind Parse. We've used it as an opportunity to rethink the ways we write and ship code, meaning we can bring you new features faster. Even from our earliest whiteboard discussions, we focused on building a new stack that would maximize our engineers' productivity for the foreseeable future. We kept in mind the past challenges our web team has faced, and designed around the layouts and interactions that we know to commonly occur in the Parse dashboard. Once we had properly framed the development goals we hoped to reach with this new web app, we picked tools and architectures that would let us achieve them. As the dashboard rolls out to the public today, we wanted to share some background behind the tools we chose, and how we built and deployed this new web app.

* * *

## Building a strong foundation with React

Picking React as a starting point for the new dashboard was a bit of a no-brainer for our team, and something we were eagerly looking forward to from the start of this project. The team's experience with it varied: some had used it since its earliest introduction within Facebook, while others had barely dipped their toes in. Above all, its focus on simple components and declarative design made it easy for everyone to dive right into development.

Most importantly, it was a tool that directly solved our real problems: React simplifies the process of building complex interfaces with many possible internal states. The Push Composer is a solid example of a form that benefits from writing declarative UI code. As options are toggled, other rows appear, disappear, and change. This scenario can be a nightmare to handle when directly toggling the visibility of various DOM nodes, and requires rigorous testing. When your DOM is guaranteed to be a function of some internal state, though, many of these concerns evaporate.

* * *

## Improving productivity with Babel

This past year saw a major shift in the way we write JavaScript at Parse. We released [Parse+React](https://github.com/ParsePlatform/ParseReact) and our new [JS client SDK](https://github.com/ParsePlatform/Parse-SDK-JS) as ES6-first projects, leveraging the power of Babel to compile down to a syntax with wider support. We carried this workflow into the development of the new dashboard so that we could continue to benefit from the productivity gains we'd experienced in previous projects. From a strict coding perspective, the new language features bring a number of time-saving idioms whose virtues have already been extolled by countless blog posts. If you haven't given it a chance yet, the experience of writing stateless React components as fat-arrow functions is surprisingly uplifting. For our team, though, the biggest value comes from a more permanent shift in philosophy: adopting Babel as a key piece of your build pipeline allows you to write code without time spent worrying about browser support or polyfills. Even when ES6 is supported on every device, this will still be true. There will always be new features with questionable support. Whether they are early TC39 proposals or custom language extensions built by ambitious teams, it will be possible to write code in the syntax of your choice.

* * *

## Disarming footguns with CSS Modules

In any large project, development issues like CSS naming conflicts often rear their ugly heads. In the past, we've attempted to fight these with varying degrees of success. As we dove into this project, we opted to try a new approach. Rather than tack on an artificial segmentation system like BEM or SUIT, we took a chance on CSS Modules. With the modular approach, our team explicitly references the CSS classes they need from within their JS code.

<pre class="line-numbers"><code class="language-javascript">import styles from 'components/MyComponent/MyComponent.css';

const MyComponent = (props) =&gt; (
  &lt;div className={styles.wrapper}&gt;
    { /* ... */ }
  &lt;/div&gt;
);</code></pre>

The immediate impact? Each component's CSS lives in an isolated namespace, and our JS code contains an explicit map of which components rely on which classes. We're hoping this second point will prove valuable in the future, enabling us to build tools that automatically detect when CSS is no longer referenced in code.

Adopting any new innovation into your stack carries with it some level of risk, but we saw this choice as the perfect bridge between old and new. At the end of the day, we're still writing CSS, which is what our team is used to. However, our styles now essentially live in our JS code. This opens us up to a number of exciting possibilities, the biggest of which might involve implementing all styles as inline JS one day.

* * *<figure id="attachment_3890" style="width: 1440px" class="wp-caption alignnone">

<img class="size-full wp-image-3890" src="{{ site.url }}/assets/wp-content/uploads/2015/12/Desktop-HD.jpg" alt="The new Parse Dashboard" width="1440" height="800" srcset="{{ site.url }}/assets/wp-content/uploads/2015/12/Desktop-HD.jpg 1440w, {{ site.url }}/assets/wp-content/uploads/2015/12/Desktop-HD-300x167.jpg 300w, {{ site.url }}/assets/wp-content/uploads/2015/12/Desktop-HD-1024x569.jpg 1024w, {{ site.url }}/assets/wp-content/uploads/2015/12/Desktop-HD-875x486.jpg 875w" sizes="(max-width: 1440px) 100vw, 1440px" /><figcaption class="wp-caption-text">The new Parse Dashboard</figcaption></figure> 

## Enjoy what we've built

Ready to get started? The next time you log in to Parse.com, you'll get a chance to switch to this new experience. Once you do, it's available anytime at [dashboard.parse.com](https://dashboard.parse.com). Keep in mind, this is a beta preview, so we value your feedback. If you see something odd or broken, let us know: just hover over the logo in the top-left to access the feedback dialog.

Implementing this new dashboard was an incredible learning experience for all of us involved, and this post just scrapes the surface of the technical and architectural decisions we made along the way. As time goes by, we'll continue to share the lessons we learned and the experiments we're attempting, right here on the Parse Blog. We're looking forward to being able to share our processes with you, the developer community.