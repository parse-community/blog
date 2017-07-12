---
id: 2548
title: Let the Browser Handle Your UI Logic for You
date: 2014-11-10T18:41:22+00:00
author: Andrew Imm
layout: post
guid: http://blog.parse.com/?p=2548
permalink: /learn/let-the-browser-handle-your-ui-logic-for-you/
post_format:
  - basic
dsq_thread_id:
  - "3612820040"
categories:
  - Learn
---
At Parse, we’re in the middle of refreshing our frontend stack, and much of that has meant reevaluating the way we write browser-bound code. We’ve begun developing new custom inputs such as date pickers, dropdowns, or toggles in a way that leverages default browser behaviors to let us write less JavaScript (and reduce opportunities for bugs). Nearly every web developer has seen or implemented a date input that looked like a calendar, and chances are it contained plenty of logic to ensure that only one day could be selected at a time. This is the exact type of logic we’re trying to let the browser handle as we rebuild our interfaces.

<h2 style="margin-bottom: 20px;">
  It all starts with a simple checkbox
</h2>

Let’s consider the scripting that might go into implementing a checkbox-style switch. When we click a switch that’s “off,” we change its state to “on” and get some visual feedback. We’d probably store its state in a boolean variable somewhere, add a click handler that updates the boolean, and then add or remove CSS classes based upon its current value. From a logic standpoint, it’s a duplication of something the browser can already do with `<input type="checkbox">`, but we put up with it for years because it let us have pretty switches.

<p style="text-align: center;">
  <img class="wp-image-2549 size-full aligncenter" src="{{ site.url }}/assets/wp-content/uploads/2014/10/uie_blog_1.png" alt="" width="275" height="100" />
</p>

Then the technique of [using checked state to style checkboxes](http://www.wufoo.com/guides/custom-radio-buttons-and-checkboxes/) hit the mainstream. Using a combination of new CSS selectors and older browser behavior, it was possible to have visual elements that were tied to invisible inputs without writing a single line of JS. We use this same technique for many of our basic toggle switches today, but we’ve also taken it to new extremes with many of our UI components.

<h2 style="margin-bottom: 20px;">
  When building components, examine their core behavior
</h2>

When we build new UI components for Parse.com, we follow a simple strategy: use the most similar browser element as a starting point. This goes beyond the obvious cases — such as using an anchor tag for something the user clicks — and looks at the inner behavior of a complex element. Remember the date picker we discussed earlier? The core interaction relies on being able to select a single day out of the month and visually represent that selection. Your browser already knows how to handle this: it’s behaviorally identical to a set of radio buttons! In fact, you’d be amazed by how many complex elements boil down to the radio button’s select-one-of-many behavior. They’re at the core of our own date picker, our fully styled dropdown menus, and a number of other switches and toggles. Knowing that the browser will ensure a single element is selected at any time allows us to eliminate a concern from our client logic.

<p style="text-align: center;">
  <img class="aligncenter size-full wp-image-2551" src="{{ site.url }}/assets/wp-content/uploads/2014/10/uie_blog_2.png" alt="uie_blog_2" width="430" height="325" />
</p>

Simultaneously, we avoid having to synchronize our data with our visualization, because a single interaction within the browser updates both. Along those same lines, at Parse we refrain from storing any data within a component that could be reasonably derived from its inner elements. For our specialized numeric inputs, we get the value by parsing the contents of an inner text input on demand, rather than returning an in-memory value that we update with an `onChange` handler. This guarantees that our JS component and its visual appearance never get out of sync.

<h2 style="margin-bottom: 20px;">
  Enough talk, let’s see a demo
</h2>

My favorite implementation of this technique is found in our Hosting product. It’s a file tree that features complex interactions with absolutely no JavaScript.

<p style="text-align: center;">
  <img class="aligncenter size-full wp-image-2552" src="{{ site.url }}/assets/wp-content/uploads/2014/10/uie_blog_3.png" alt="uie_blog_3" width="420" height="240" />
</p>

When I designed the tree, I broke it down into its core behaviors: it has folders that open and close to display contents, and an overall set of files that can be individually selected. If you’re following along, it should make sense that the folders are backed by checkboxes while the files themselves are a single set of radio buttons.

<pre class="EnlighterJSRAW" data-enlighter-language="html">&lt;!-- Files are a set of radio buttons with the same name field --&gt;
&lt;input type="radio" name="hosted_files" id="f_myfile_js" value="MyFile.js"&gt;
&lt;label for="f_myfile_js"&gt;MyFile.js&lt;/label&gt;

&lt;input type="radio" name="hosted_files" id="f_another_js" value="Another.js"&gt;
&lt;label for="f_another_js"&gt;Another.js&lt;/label&gt;

&lt;!-- Folders are checkboxes that toggle the visibility of divs --&gt;
&lt;input type="checkbox" id="f_myfolder"&gt;
&lt;label for="f_myfolder"&gt;My Folder&lt;/label&gt;
&lt;div class="dir_wrapper"&gt;
  Folder contents...
&lt;/div&gt;</pre>

In CSS, we simply provide default styles for labels, and additional styles for when they follow a checked input.

<pre class="EnlighterJSRAW" data-enlighter-language="css">label {
  color: #ffffff;
}
input[type=radio]:checked + label {
  color: #5298fc;
}
</pre>

For folders, we also toggle the display property of a `.dir_wrapper` element that comes after a selected checkbox.

<pre class="EnlighterJSRAW" data-enlighter-language="css">.dir_wrapper {
  display: none;
}

input[type=checkbox]:checked + label + .dir_wrapper {
  display: block;
}</pre>

With these styles, I don’t need to worry about having numerous click handlers to open or close folders, and a single change handler on the radio buttons can tell me when a new file is selected.

You can examine a slightly modified version of our file tree at [this CodePen](http://codepen.io/andrewi-fb/pen/DBulF).

<h2 style="margin-bottom: 20px;">
  So what?
</h2>

As web developers, we tend to lean a lot on JavaScript to build fancy interfaces and effects. I know I certainly do. But the next time you set out to build something new, look at its core behaviors and ask yourself if there is some pre-written, [standardized](http://www.w3.org/TR/html5/) part of the web browser that can handle some of the work for you. Yes, there is a bit of a tradeoff with markup instead of code, but I'd rather write code to generate HTML than deal with synchronizing data and visualization, handling a plethora of clicks, taps, and keyboard inputs, and ultimately replicating behavior that's already embedded in the browser.

With these methods, you can build custom inputs that play nicely with the existing web. They fire native change events, they don't risk side-effects in your view logic, and in many cases they even improve screen reader accessibility. If you take a look at the [HTML5 spec for radio buttons](http://www.w3.org/TR/html5/forms.html#radio-button-state-(type=radio)), you'll see that it only talks about behavior, not appearance. It's no long stretch to reuse that behavior in new and creative ways, and I look forward to seeing how others employ it for interface construction.