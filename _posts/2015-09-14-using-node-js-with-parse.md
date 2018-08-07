---
id: 3798
title: Using Node.js with Parse
date: 2015-09-14T16:13:20+00:00
author: foscomarotto
layout: post
guid: http://blog.parseplatform.org/?p=3798
permalink: /learn/using-node-js-with-parse/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4130079743"
categories:
  - Engineering
  - Learn
tags:
  - cloudcode
  - express.js
  - node.js
---
The Cloud Code environment is the fastest way to get some dynamic application logic running online. But as your app grows, you may want more features, like the ability to use arbitrary node.js modules or to test things locally. The Parse Hooks API makes it possible to use your own node.js server in conjunction with Parse, and today we're launching functionality to make it even easier.

* * *

## Adding Node.js to a Parse App

If your app isn't using Cloud Code yet, it's pretty straightforward to add node.js server-side logic instead of using our hosted Cloud Code environment. We've created an Express middleware that offers the Parse.Cloud namespace, and produces an Express app which validates requests and has working routes for each trigger and function. Check it out on [GitHub](https://www.npmjs.com/package/parse-cloud-express).

For example, if you already have a Parse app that isn't using Cloud Code, this simple script will have you serving a cloud function from node.js:

<pre class="line-numbers"><code class="language-java">
var http = require('http');
var express = require('express');
var bodyParser = require('body-parser');
var ParseCloud = require('parse-cloud-express');
var Parse = ParseCloud.Parse;

var app = express();

// Host static files from public/
app.use(express.static(__dirname + '/public'));

// Define a Cloud Code function:
Parse.Cloud.define('hello', function(req, res) {
  res.success('Hello from Cloud Code on Node.');
});

// Mount the Cloud Code routes on the main Express app at /webhooks/
// The cloud function above will be available at /webhooks/function_hello
app.use('/webhooks', ParseCloud.app);

// Launch the HTTP server
var port = process.env.PORT || 5000;
var server = http.createServer(app);
server.listen(port, function() {
  console.log('Cloud Code on Node running on port ' + port + '.');
});
</code></pre>

&nbsp;

## Switching from Cloud Code to Node.js

Most but not all Cloud Code can be directly ported to node.js. Parse Cloud Code is based on the V8 JavaScript engine, but it does not run node.js. In particular, Cloud Code environments are loaded and unloaded individually for each request, while node.js services typically serve multiple requests at a time. So if your code relies on global variables, you'll need to refactor that before using node.js.

Also in Cloud Code, the concept of a method that returns the current user makes sense, as it does in JavaScript on a web page, because there's only one active request and only one user. However in a context like node.js, there can't be a global current user, which requires explicit passing of the session token. Version 1.6 and higher of the Parse JavaScript SDK already requires this, so if you're at that version, you're safe in your library usage.

<pre class="line-numbers"><code class="language-java">
Parse.Cloud.define('findBacon', function(req, res) {
var token = req.user.getSessionToken();
var query = new Parse.Query('Bacon');
// Pass the session token to the query
query.find({ sessionToken: token }).then( ... );
});
</code></pre>

## Local Testing

Using node.js makes it easier to test your changes locally, since you can run a full node.js environment on your local machine.

Here is a way to test locally using [ngrok](https://ngrok.com/) and the Hooks API. Ngrok allows you to expose a service running on your local machine, through a public URL protected by HTTPS. If you run an HTTP server on your local machine listening on port 5000, you can then run `ngrok http 5000` and configure your webhooks using the provided URL. It is also possible to [debug your cloud code](https://www.jetbrains.com/webstorm/help/running-and-debugging-node-js.html) while it runs.

<img src="{{ site.url }}/assets/wp-content/uploads/2015/09/2__ngrok-1024x456.png" alt="2__ngrok" width="640" height="285" class="aligncenter size-large wp-image-3812" srcset="{{ site.url }}/assets/wp-content/uploads/2015/09/2__ngrok-1024x456.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/09/2__ngrok-300x133.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/09/2__ngrok-875x389.png 875w, {{ site.url }}/assets/wp-content/uploads/2015/09/2__ngrok.png 1142w" sizes="(max-width: 640px) 100vw, 640px" />

Once you've got ngrok running, you can now configure your webhooks with Parse. This can be done from the dashboard, or through the Hooks API. We've created an example project that contains a script that will automatically register your webhooks. Check out the [repository](https://github.com/ParsePlatform/CloudCode-Express), and here's how you'd use [the script](https://github.com/ParsePlatform/CloudCode-Express/blob/master/scripts/register-webhooks.js) in this example:

<pre class="line-numbers"><code class="language-java">
export HOOKS_URL=https://032b47d7.ngrok.io/
npm run register</code></pre>

* * *

We want Parse apps to work just as smoothly whether you're using our hosted Cloud Code environment or whether you're developing in node.js. Try it out if you are interested in the extra functionality of node.js or local development and let us know how it goes!