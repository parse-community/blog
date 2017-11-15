---
id: 3737
title: The Joy of Weekend Hacking
date: 2015-08-26T11:07:46+00:00
author: foscomarotto
layout: post
guid: http://blog.parseplatform.org/?p=3737
permalink: /learn/the-joy-of-weekend-hacking/
post_format:
  - feat-image
app_store_link_id:
  - ""
hide_from_index:
  - "0"
feature_image_style:
  - large
dsq_thread_id:
  - "4069442758"
image: /wp-content/uploads/2015/08/weekend-hacking-hero.jpg
categories:
  - Engineering
  - Learn
tags:
  - heroku
  - twilio
  - weekendhacking
---
There's really nothing like getting an idea on Friday, and having a working prototype by Monday. Many companies are started this way, and it's what brings tens of thousands of people to hackathons every year. Parse does an incredible job in this space, offering a host of services which are easy to integrate. I want to tell you about how I built a fully-realized <a href="https://en.wikipedia.org/wiki/Minimum_viable_product" target="_blank">MVP</a> last weekend using [Parse](https://parse.com), [Heroku](https://heroku.com), and [Twilio](http://twilio.com), while fitting in an overnight vacation to Santa Cruz.

* * *

## The idea

In 2011, I sold two local restaurants on a text messaging service. They would advertise and encourage their customers to text a keyword to a phone number, which would subscribe them to updates from that restaurant. The restaurant could then use a web application to send messages out to their subscribers. This worked for a while, with one restaurant paying for the service for three years and religiously sending out a weekly special every Monday.

I've thought about it many times since then, but Friday I came home with a mission, to make a fully automated version of this and not to worry about the billing aspect for now. Let people register a code, get subscribers, and send out text and picture messages to their followers, all from their phone without an app. I can think of a lot of scenarios where this would be a great solution, like event photo sharing, or live-stream events.
  
&nbsp;
  
<img src="{{ site.url }}/assets/wp-content/uploads/2015/08/skipcode1.jpg" alt="skipcode1" width="1200" height="800" class="alignleft size-full wp-image-3745" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/skipcode1.jpg 1200w, {{ site.url }}/assets/wp-content/uploads/2015/08/skipcode1-300x200.jpg 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/skipcode1-1024x683.jpg 1024w, {{ site.url }}/assets/wp-content/uploads/2015/08/skipcode1-875x583.jpg 875w" sizes="(max-width: 1200px) 100vw, 1200px" />

* * *

## 

## Preparation

You don't show up to a hackathon with a brand-new laptop without any software. You come prepared, with your development environment ready to go. Text editors and IDEs, revision control software, terminal emulators, and package managers are all things you're likely to have installed. These were my pre-requisites for last weekend:

### [Node.js](http://nodejs.org)

I've been using it since 2011, and it's currently my default platform for most projects. It comes with [npm](http://npmjs.org), a great package management system, which is useful for more than just downloading modules.

### [Parse](https://parse.com) 

Of course I have a Parse account and the Parse [Command Line Tool](https://parse.com/apps/quickstart/#cloud_code) installed. There's still no easier way to store and retrieve some data than by using a Parse SDK.

### [Twilio](https://twilio.com)

I have an account with Twilio and several phone numbers. They offer a simple and affordable API for integrating SMS and MMS with any project.

### [Heroku](https://heroku.com)

I've always got the [Heroku Toolbelt](https://toolbelt.heroku.com/) installed and ready for when I want to build something. It's great being able to quickly deploy a service to a public-facing server, protected via SSL, and have it configured automatically.

### [Git](http://git-scm.com/)

One of the most popular revision control systems, and the only one I really care to use. Plus, when I'm using a Windows machine, I really like the Git Bash terminal.

### [Visual Studio Code](https://code.visualstudio.com/)

My new favorite text editor for projects like this. I just open a folder and start working.

* * *

## 

## Architecture

All of the input and output for this project will be through Twilio. Text and picture messages will come in to my Twilio number, which will trigger a webhook in my application logic, and text and picture messages will be sent out using the `twilio` module.

I want to run this service on Heroku, instead of just using Parse Cloud Code, because I want the full capabilities of Node.js and easy access to all of the modules in npm.

I chose Parse to store the codes and the follower connections between phone numbers. I've not found a better fit for this exact scenario. I just want to persist some data, and get it back out, with the least amount of setup and administration possible. The workflow of deploying apps with Heroku and Parse is incredibly easy.

### Dependencies and npm

Tools are getting very good at helping us manage dependencies. For Node.js projects, I started with a template file:

<pre class="line-numbers"><code class="language-javascript">{
  "name": "",
  "version": "",
  "description": "",
  "author": "",
  "scripts": {},
  "repository": {},
  "dependencies": {},
  "license": ""
}</code></pre>

I filled it in with some basic details and project dependencies, then saved it as `package.json`:

<pre class="line-numbers"><code class="language-javascript">{
  "name": "skipcodes",
  "version": "0.1.0",
  "description": "Broadcast SMS and MMS to subscribers",
  "author": "Fosco Marotto &lt;thedude@fb.com&gt;",
  "scripts": {
    "start": "node server.js"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/gfosco/skipcodes.git"
  },
  "dependencies": {
    "parse": "~1.5",
    "twilio": "~2.3",
    "express": "~4.13",
    "underscore": "~1.8",
    "body-parser": "~1.13"
  },
  "license": "proprietary"
}</code></pre>

Running `npm install` will download and install everything to the `node_modules` folder. I based this project on the public `parse`, `twilio`, `express`, `underscore`, and `body-parser` modules.

### Git

I initialized the project with `git init` and whenever I had changes to deploy, I ran `git add .` and `git commit -m "Some description of the update"`. Knowing more about working in Git would be valuable for any developer, as it's not always this simplistic.

### Heroku

Heroku sets itself up as a remote Git repository for your project. Make sure you're logged in with `heroku login` and then create a new Heroku app for the project by running `heroku create`. When created, you'll get the URL for your Heroku app. Use this to set up your Twilio webhooks.

When you're ready to deploy your code, commit everything and run `git push heroku`.

### Twilio

I rented a phone number through Twilio and configured it using my Heroku app URL. I set up Twilio to POST all inbound SMS/MMS to `https://some-instance-1234.herokuapp.com/twiml`:
  
&nbsp;
  
<img src="{{ site.url }}/assets/wp-content/uploads/2015/08/twilio-config-updated.png" alt="twilio-config-updated" width="1622" height="470" class="alignnone size-full wp-image-3746" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/twilio-config-updated.png 1622w, {{ site.url }}/assets/wp-content/uploads/2015/08/twilio-config-updated-300x87.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/twilio-config-updated-1024x297.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/08/twilio-config-updated-875x254.png 875w" sizes="(max-width: 1622px) 100vw, 1622px" />
  
&nbsp;
  
They have a great logs system, so during development you can see which requests failed, and the error messages are links to more detailed information and request/response inspection.

* * *

&nbsp;

## Iterate, iterate, iterate

There are many development styles, none of which is the right way, but my personal favorite for weekend hacking is to write some code, deploy it, and see what happens. Then do that again, and again, learning something new each time, until you're done.

Friday night I started setting up the basics in `server.js`:

<pre class="line-numbers"><code class="language-javascript">var http = require('http');
var twilio = require('twilio');
var express = require('express');
var bodyParser = require('body-parser');

var twilioSid = 'some twilio sid';
var twilioToken = 'some twilio auth token';
var twilioClient = require('twilio')(twilioSid, twilioToken);

var app = express();
app.use(bodyParser.urlencoded({
  extended: false
}));

// Twilio will POST to this route when SMS/MMS is received
app.post('/twiml', function(req, res) {
  // Log the data sent by Twilio
  console.log(req.body);
  messageResponse('Hello World.', res);
});

// Launch the HTTP server serving the Express app
http.createServer(app).listen(process.env.PORT);

// Respond with TwiML indicating an SMS reply message
function messageResponse(message, res) {
  var resp = new twilio.TwimlResponse();
  resp.message(function() {
    this.body(message);
  });
  res.type('text/xml');
  res.send(resp.toString());
}</code></pre>

I saved and committed my work, pushed it to Heroku, and grabbed for my phone. "Test" is what I sent in, and "Hello World." came back quickly. By starting with the simplest scenario, and building up, you're constantly experiencing small successes and the positive feelings they provide.

I ran `heroku logs` and inspected the data from Twilio, identifying which fields I needed to look at. For this project, I use `From`, `Body`, `NumMedia`, and `MediaUrl*`.

As I started building out functionality, I kept iterating, logging things, texting and testing over and over. When I finished up on Sunday night, `server.js` was 350 lines, and I had 57 commits.

* * *

&nbsp;

## The Server Stays Running

Usually you build a service that starts up, initializes itself, and then handles requests forever unless it crashes. This is a major difference between using Node.js on Heroku, compared to Parse Cloud Code where the environment is stateless and new for every request. In this case, I know there will only be a single server responding to requests, so I decided to load all the data from the database on startup. When changes are made, I update the local instance of the object and save it back to Parse.

I wrote something like this:

<pre class="line-numbers"><code class="language-javascript">console.log('Loading Codes database...');
var CODES = {};
var query = new Parse.Query("Codes");
query.find({ useMasterKey: true }).then(function(codes) {
  var ct = 0;
  _.each(codes, function(code) {
    ct++;
    var codeName = code.get('code').toLowerCase();
    CODES[codeName] = code;
  });
  console.log('Loaded ' + ct + ' codes.');
});</code></pre>

I now had a global list of all the objects in the Codes class (up to 100 by default) indexed by the lower-cased version of the text code. When an SMS comes in with that code, I can easily relate it to the `Parse.Object`. In the Parse Data Browser, the Codes class looks like this:
  
&nbsp;
  
<img src="{{ site.url }}/assets/wp-content/uploads/2015/08/code-browser-final.png" alt="code-browser-final" width="1344" height="156" class="alignnone size-full wp-image-3747" srcset="{{ site.url }}/assets/wp-content/uploads/2015/08/code-browser-final.png 1344w, {{ site.url }}/assets/wp-content/uploads/2015/08/code-browser-final-300x35.png 300w, {{ site.url }}/assets/wp-content/uploads/2015/08/code-browser-final-1024x119.png 1024w, {{ site.url }}/assets/wp-content/uploads/2015/08/code-browser-final-875x102.png 875w" sizes="(max-width: 1344px) 100vw, 1344px" />
  
&nbsp;
  
I did the same thing with the `Phones` class, indexing them by the `From` parameter containing the phone number.

* * *

&nbsp;

## Optimistic updates

This is a weekend project, it doesn't need to be bullet-proof. During the weekend, I wrote several functions like this:

<pre class="line-numbers"><code class="language-javascript">function addFollowerToCode(phoneTo, codeObj) {
  codeObj.addUnique('followers', phoneTo);
  codeObj.save({}, { useMasterKey: true }).then(function() {
    console.log('codeObj saved.');
  }, function(err) {
    console.log('codeObj save failure.');
    console.log(err);
  });
}</code></pre>

If for some reason this action failed, the user wouldn't be notified and I would have to check the logs to find it. You can call this technical debt, because if the project becomes a huge success this will need to be re-written.

* * *

&nbsp;

## Sending messages

Once I had worked out the code system, allowing people to register codes and others to subscribe to them, my final task was to allow the owner of a code to broadcast text and picture messages to their followers. This is the function for sending plain-text messages:

<pre class="line-numbers"><code class="language-javascript">function sendMessageToCode(msgBody, codeObj) {
  var followers = codeObj.get('followers');
  if (followers && followers.length) {
    var ct = 0;
    _.each(followers, function(target) {
      ct++;
      twilioClient.sendMessage({
        'to': target,
        'from': '+15512231337',
        'body': codeObj.get('code') + ': ' + msgBody
      }, function(err, response) {
        console.log(err);
      });
    });
    console.log('Sent ' + ct + ' messages for ' + codeObj.get('code') + '.');
  }
}</code></pre>

Sending MMS messages was only slightly more complicated, as I send them individually, but most of the code is identical. Before the weekend was over, I had the whole process working... registering a code with a password, subscribing to codes, unsubscribing from codes, logging in with a code and password, and broadcasting SMS/MMS from an authenticated user to all of their subscribers.
  
&nbsp;
  
<img src="{{ site.url }}/assets/wp-content/uploads/2015/08/broadcast-recieve1.jpg" alt="broadcast-recieve" width="1200" height="744" class="alignnone size-full wp-image-3748" />

* * *

## 

## Launching a weekend project

When you turn an idea into code you often want to show it to other people, and learn from their reactions and feedback. You can post on your personal social media accounts, create new ones just for your project, or... why not both? Mention related figures or brands, see if you can get re-shared to larger audiences, even invest a few dollars to promote your post. For this project, I did [all](https://twitter.com/newFosco/status/635729753312354304) [of](https://twitter.com/SkipCodesMMS) [that](https://twitter.com/twilio/status/635926173709529089).

<blockquote class="twitter-tweet" lang="en">
  <p lang="en" dir="ltr">
    .<a href="https://twitter.com/newFosco">@newFosco</a> Weekend hacks FTW. Thanks for sharing your build <a href="https://twitter.com/SkipCodesMMS">@SkipCodesMMS</a>, text "Fosco" to 551-223-1337 or "Hi" to learn how get your own.
  </p>
  
  <p>
    &mdash; twilio (@twilio) <a href="https://twitter.com/twilio/status/635926173709529089">August 24, 2015</a>
  </p>
</blockquote>


  
Now, this is just one story of Weekend Hacking... What's yours? Send your story to <community@parse.com> and maybe you can write the next edition of Weekend Hacking. Happy Building!