---
id: 3517
title: Our Docs Are Now Open Sourced
date: 2015-05-13T12:49:47+00:00
author: mattieugamacheasselin
layout: post
guid: http://blog.parseplatform.org/?p=3517
permalink: /announcements/our-docs-are-now-open-sourced/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - large
dsq_thread_id:
  - "3763856723"
image: /wp-content/uploads/2015/05/docs-hero.jpg
categories:
  - Announcements
tags:
  - docs
  - opensource
---
Documentation is often one of the most overlooked components of a developer product. It's also one of the most crucial: No matter how powerful a new platform may be, developers won't know how to use it without clear documentation.

Here at Parse, we've always been focused on making it incredibly easy for you to get started with the basics, and to quickly learn how to utilize more advanced features. A big part of this effort has manifested in our <a href="https://parse.com/docs" target="_blank">guides</a>.

* * *

## New Look and Feel

As you may have noticed, we recently launched a brand new look and feel for these guides — but that's not the only change we've made.<figure id="attachment_3524" style="width: 2756px" class="wp-caption alignnone">

<img class="wp-image-3524 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/05/parse-docs.jpg" alt="" width="2756" height="1300" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/parse-docs.jpg 2756w, {{ site.url }}/assets/wp-content/uploads/2015/05/parse-docs-300x142.jpg 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/parse-docs-1024x483.jpg 1024w, {{ site.url }}/assets/wp-content/uploads/2015/05/parse-docs-875x413.jpg 875w" sizes="(max-width: 2756px) 100vw, 2756px" /><figcaption class="wp-caption-text">We've updated the look of our docs!</figcaption></figure> 

## Open Source Docs

Today, we're pleased to share a new, open-source system for editing and maintaining our guides, so you can help us keep them accurate and up-to-date.

Over the years, many fixes to our docs have stemmed from bug reports from our community. Since we don't use a CMS for our guides, making changes requires an engineer to write code and then deploy it. This process hasn't been ideal for fixing a simple typo or adding a small point of clarification. We considered switching to one of the many CMS options, but we've always found that these didn't allow us to offer the best possible user experience to our developers.

So, we built our own solution using GitHub and Parse. All of our guides are now available in markdown at <https://github.com/ParsePlatform/Docs>. When we want to deploy the latest changes, we run a <a href="https://parse.com/docs/js/guide#cloud-code-advanced-background-jobs" target="_blank">Background Job</a> in Cloud Code that pulls all of the markdown files from GitHub, processes them, and stores the HTML in a Parse app. Then, when you're visiting one of our guide pages, we pull the content from Parse and show it all on the webpage.<figure id="attachment_3523" style="width: 2850px" class="wp-caption alignnone">

<img class="wp-image-3523 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/05/Screen-Shot-2015-05-13-at-2.46.08-PM.png" alt="" width="2850" height="1562" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/Screen-Shot-2015-05-13-at-2.46.08-PM.png 2850w, {{ site.url }}/assets/wp-content/uploads/2015/05/Screen-Shot-2015-05-13-at-2.46.08-PM-300x164.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/Screen-Shot-2015-05-13-at-2.46.08-PM-1024x561.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/05/Screen-Shot-2015-05-13-at-2.46.08-PM-875x480.png 875w" sizes="(max-width: 2850px) 100vw, 2850px" /><figcaption class="wp-caption-text">Help improve our docs via GitHub.</figcaption></figure> 

If you find any mistakes or have ideas on how to make our docs better, you can submit a pull request directly from GitHub and we'll have the change deployed live in no time. We hope this new approach will help us continue to improve the quality of our docs by allowing all of you to share your opinions for making the guides even better.

* * *

So, have a suggestion? Hit the “Edit this section” link at the end of any section of our docs and send us a pull request now!