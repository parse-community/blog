---
id: 3613
title: Improving the Parse Command Line Tool
date: 2015-06-17T08:41:03+00:00
author: pavanathivarapu
layout: post
guid: http://blog.parseplatform.org/?p=3613
permalink: /announcements/improving-the-parse-command-line-tool/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3856874593"
categories:
  - Announcements
tags:
  - cloudcode
  - commandline
  - improvements
---
In March, we launched a new version of the [Command Line Tool](https://parse.com/docs/js/guide#command-line), which was completely rewritten in Go. The main motivation behind the Go rewrite was to leverage Go's cross platform capabilities: Now, we can generate static binaries for Linux, Mac, and Windows from a single codebase. These static binaries are of the same download size as the previous version of the Command Line Tool, which is great considering that they work off the shelf and don't need a Python interpreter to be installed on the operating system.

**Today, we're releasing a new version of the Command Line Tool with several improved features.** In this new version, the output of the “deploy” and “releases” command has more useful information, and “parse deploy” now prints the names of all files that have been deployed. 

<pre class="line-numbers"><code class="language-bash">[myapp]$ parse deploy 
Uploading source files 
Uploading recent changes to scripts... 
The following files will be uploaded: 
..... 
Uploading recent changes to hosting... 
The following files will be uploaded: ..... 
Finished uploading files 
New release is named v1 (using Parse JavaScript SDK vx.x.x)</code></pre>

Using the “-v” or “--version” option for the “releases” command, you can now view the list of all files that were deployed in a given release.

<pre class="line-numbers"><code class="language-bash">[myapp]$ parse releases -v v1
Deployed cloud code files: 
main.js 
Deployed public hosting files: 
index.html</code></pre>

Also, you can now run Parse commands from any directory within the Parse project. For instance:

<pre class="line-numbers"><code class="language-bash">[myapp/cloud]$ parse deploy
Uploading source files
.....</code></pre>

The Command Line Tool now updates automatically to the latest stable.

Additionally, the tool can now auto-correct spelling mistakes in Parse commands. This saves you from the hassle of having to type the entire command again just because of one unseen typo.

For instance, 

<pre class="line-numbers"><code class="language-bash">[myapp]$ parse depluy
(assuming by `depluy` you meant `deploy`)
.... </code></pre>

**If you're still using version 1.4.4 of the Command Line Tool, now is the time to [upgrade](https://parse.com/apps/quickstart#cloud_code) and take advantage of the tool's new features.** Check out the documentation [here](https://parse.com/docs/js/guide#command-line), and if you have any questions or feedback, please reach out to us [here](https://parse.com/help).