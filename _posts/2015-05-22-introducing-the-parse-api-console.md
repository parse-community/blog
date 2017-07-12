---
id: 3576
title: Introducing the Parse API Console
date: 2015-05-22T09:34:57+00:00
author: BjörnKaiser
layout: post
guid: http://blog.parse.com/?p=3576
permalink: /announcements/introducing-the-parse-api-console/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3785916235"
categories:
  - Announcements
tags:
  - APIs
  - core
---
Here at Parse, we love building tools to speed up tasks and simplify processes, freeing your time so you can focus on creating an app with the best possible user experience. In that spirit, today we're excited to add the Parse API Console to the Parse development toolbox. **With the API Console, __you have quick and easy access to all functions of our REST API, allowing you to play around with the API or debug issues without writing a single line of code** — so you can ultimately build your apps faster.

The new tool has its origins within our Developer Support Team, who handle customer questions and issues with the Parse Platform. They initially built the Console as an internal tool allowing them to more easily explore the API and tackle issues — But we thought some of you might benefit from it as well, so we built a public version.

Previously, if you wanted to reproduce an issue you were seeing with a request, or try out an API feature, you'd most likely write client-side code or manually construct a cURL command. But that can be a laborious process which takes away valuable time that you could be putting toward designing great experiences for your users.

With the Parse API Console, it's now just a matter of a few clicks to run queries and experiment with our API.  And to make it even easier for you, you can export all requests from the API Console to cURL commands.

<img class="alignnone wp-image-3577" src="{{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-1-1024x570.png" alt="API Console 1" width="640" height="356" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-1-1024x570.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-1-300x167.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-1-875x487.png 875w" sizes="(max-width: 640px) 100vw, 640px" />

Here are a few things you can do with the Parse API Console:

<ul class="standard-list">
  <li>
    Fetch, create, update, and delete objects
  </li>
  <li>
    Run requests as a specific user of your app (to test your ACLs and CLPs)
  </li>
  <li>
    Call your Cloud Code functions and start background jobs
  </li>
  <li>
    Export calls to cURL
  </li>
</ul>

It's easy to make your first request — you select the endpoint of the API you want to hit, optionally enable usage of the master key or a user to run the request as, provide the body data, and off you go. The results will be displayed immediately for you to inspect.

<img class="alignnone wp-image-3578 size-large" src="{{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-2-1024x606.png" alt="API Console 2" width="640" height="379" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-2-1024x606.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-2-300x178.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/05/API-Console-2-875x518.png 875w" sizes="(max-width: 640px) 100vw, 640px" />

We hope this tool will speed up your work with Parse by giving you better insight into how our APIs work. If you have any feedback or feature requests, let us know [here](https://groups.google.com/forum/#!forum/parse-developers). We're always working on tools to make your life as a developer easier — so stay tuned for more!