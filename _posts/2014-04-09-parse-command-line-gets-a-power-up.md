---
id: 2120
title: Parse Command Line Gets a Power Up
date: 2014-04-09T17:06:58+00:00
author: Listiarso Wastuargo
layout: post
guid: http://blog.parse.com/?p=2120
permalink: /announcements/parse-command-line-gets-a-power-up/
dsq_thread_id:
  - "3688172349"
categories:
  - Announcements
  - Engineering
---
As more people use Cloud Code, we strive to improve the tool that directly supports it, the [Parse Command Line Tool](https://parse.com/docs/cloud_code_guide#started). Today, we're announcing three new features we've added to our command line tool: `parse update`, `parse default`, and `parse jssdk`.

`<strong>parse update</strong>`

As the name implies, this command helps you update the tool to its latest version. If you've previously updated the tool, you had to revisit the Parse docs and copy/paste this script to your terminal:

<pre class="brush: bash gutter: false">curl -s https://www.parse.com/downloads/cloud_code/installer.sh | sudo /bin/bash</pre>

This is where `parse update` comes to the rescue! Now, you can update directly from version 1.3.0 or later. This feature is currently only available for Unix-based operating systems, but we're hard at work getting it added to Windows.

`<strong>parse default</strong>`

This command returns the default app that will be targeted with our existing features, such as `parse deploy`, `parse log`, `parse releases` and `parse rollback,` when an app is not explicitly provided. Before this patch, you had to edit your config file and the `_default`'s link to your desired app if you wanted to leverage the default app behavior. Now, you can use `parse default [app name]` to set your default app.

`<strong>parse jssdk</strong>`

This feature returns the current Parse JavaScript SDK used on your Cloud Code. You can also explicitly set your current JavaScript SDK by using `parse jssdk [version]`. This allows you to have control over the Parse JavaScript SDK on your Cloud Code. You can see all available Parse JavaScript SDK versions by running `parse jssdk -a`.

<pre class="brush: bash gutter: false">$ parse jssdk -a
  1.2.17
  1.2.16
  1.2.15
  1.2.14
* 1.2.13
  1.2.12
  1.2.11
  ...</pre>

You'll find the full docs [here](https://parse.com/docs/cloud_code_guide#clt). Now, go update your command line tool and let us know what you think!