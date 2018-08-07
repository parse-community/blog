---
id: 3955
title: Introducing the Parse Server Dashboard
date: 2016-03-04T12:58:58+00:00
author: drewgross
layout: post
guid: http://blog.parseplatform.org/?p=3955
permalink: /announcements/introducing-the-parse-server-dashboard/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Announcements
---
With the winding down of the Parse hosted service and the transition to the Open Source <a href="http://blog.parseplatform.org/announcements/what-is-parse-server/" target="_blank">Parse Server</a>, many of you have been asking how you will manage your app when it’s running on your own system. Today we are answering that question with <a href="https://github.com/ParsePlatform/parse-dashboard" target="_blank">Parse Dashboard</a>. You can start using it today to manage the apps that you have already moved to Parse Server, your apps that are still on <a href="http://parse.com/" target="_blank">Parse.com</a>, and your apps that are still in development and running on Parse Server on your development machine. You can even manage them all from the same dashboard.

## Getting Started

The Parse Dashboard is adapted from the same dashboard we launched in December, and running it will feel very familiar to any Node.js or web developers. If you already have Node.js, all it takes to get started is four simple commands to clone the source code, and a little bit of info in a config file.

<pre class="line-numbers"><code class="language-bash">
git clone git@github.com:ParsePlatform/parse-dashboard.git
cd parse-dashboard
npm install
</code></pre>

Now that you have the dashboard installed, edit the parse-dashboard-config.json file in the Parse-Dashboard folder, and add your app’s ID, master key, server URL, and name, like this file:

<pre class="line-numbers"><code class="language-javascript">
{
  "apps": [
    {
      "serverURL": "http://localhost:1337/parse",
      "appId": "myAppId",
      "masterKey": "myMasterKey",
      "appName": "MyApp"
    }
  ]
}
</code></pre>

Then, execute `npm run dashboard` and visit `http://localhost:4040` in your browser, and you will see your app! For your apps on Parse.com, you can currently use the Data Browser, edit your Parse Config, use the API console, and view your Cloud Code and logs. For apps on Parse Server, the Data Browser and API console are available.

![Parse Dashboard Screenshot]({{ site.url }}/assets/wp-content/uploads/2016/03/dash-shot.png)

## Staying Up to Date

The Parse Dashboard detects which features are available on your Parse Server and enables only the features that are actually available. This makes it very easy to keep your dashboard up to date with the latest and greatest with just git pull, even if your apps on Parse Server aren’t yet using the latest version.

## Contributing

Having the Parse Dashboard be a completely separate project from the server also makes it easy to add new features to the dashboard and have them available immediately. We hope to see the Parse community take ownership of the Parse Dashboard and drive the creation of new features and distribution methods. Many of the features from <a href="http://parse.com/" target="_blank">Parse.com</a> can be supported with only some small tweaks, so we’ve made all of the code for the dashboard available, even for features that currently aren’t available in Parse Server. Happy hacking!