---
id: 2662
title: Using Parse to Power Up Your Framer Prototypes (Part I)
date: 2014-12-18T21:48:08+00:00
author: George Kedenburg III
layout: post
guid: http://blog.parse.com/?p=2662
permalink: /learn/using-parse-to-power-up-your-framer-prototypes-part-i/
post_format:
  - feat-image
feature_image_style:
  - large
dsq_thread_id:
  - "3607686915"
app_store_link_id:
  - ""
hide_from_index:
  - "0"
image: /wp-content/uploads/2014/12/Morpheus.jpg
categories:
  - Design
  - Learn
  - Tutorial
tags:
  - design
  - framer
  - prototyping
  - tutorial
---
<p class="graf--p">
  <em>At Parse, prototyping plays a crucial part of how we craft our products. Read on for a great tutorial to make your prototypes as realistic as possible with the help of Framer. Brought to you by our resident design expert, George Kedenburg III, and originally posted <a href="https://medium.com/@gk3/using-parse-to-power-up-your-framer-prototypes-88cb87009d00" target="_blank">here</a> on Medium.</em>
</p>

<p class="graf--p">
  Have you ever wished your prototypes could be just a bit more realistic? Are you tired of prototypes resetting back to their initial state when you close them? Maybe you just want an easier way to integrate real data without dealing with obnoxious blobs of JSON.
</p>

<p class="graf--p graf--last">
  <a id="f01f"></a>In this series of tutorials, I’m going to be exploring some interesting use cases for integrating <a class="markup--anchor markup--p-anchor" href="http://parse.com/" target="_blank" rel="nofollow" data-href="http://parse.com"><strong class="markup--strong markup--p-strong">Parse</strong></a> with <a class="markup--anchor markup--p-anchor" href="http://framerjs.com/" target="_blank" rel="nofollow" data-href="http://framerjs.com"><strong class="markup--strong markup--p-strong">Framer</strong></a>. Hopefully they’ll help you think of new ways to use data to enhance your own prototypes with the awesome power of a real mobile backend.
</p>

![](http://i.imgur.com/N0T93Sf.gif "source: imgur.com")

<p class="graf--p">
  In this first guide, we’re just going to go through some basic things you can do with Parse. We’ll set you up with an account, create your first app, set up a class full of data, and then pull that in to the prototype. We’ll also link up a simple “like” interaction that will save data back to Parse.
</p>

<p class="graf--p">
  <a id="ff0e"></a>Our app concept is simple: we just want to display a list of locations (that have their own background images) and store a total like count per location.
</p>

<p class="graf--p">
  <a id="632a"></a><a class="markup--anchor markup--p-anchor" href="http://framerjs.com/examples/preview/#parse-data.framer" target="_blank" rel="nofollow" data-href="http://framerjs.com/examples/preview/#parse-data.framer"><strong class="markup--strong markup--p-strong"><em class="markup--em markup--p-em">Play with the completed prototype</em></strong></a>
</p>

* * *

<h2 class="graf--h4" data-align="center">
  What is Parse?
</h2>

<p class="graf--p graf--last">
  <a id="2b70"></a>Let’s take a step back. <a class="markup--anchor markup--p-anchor" href="http://www.parse.com/" target="_blank" rel="nofollow" data-href="http://www.parse.com">Parse</a> is a <strong class="markup--strong markup--p-strong"><em class="markup--em markup--p-em">cloud platform</em></strong> that lets app developers quickly and easily integrate a lot of complicated, scalable server-side functionality into their products.
</p>

<p class="graf--p" data-align="center">
  <strong class="markup--strong markup--p-strong"><em class="markup--em markup--p-em">Parse has three main products:</em></strong>
</p>

<p class="graf--h4" data-align="center">
  <a class="markup--anchor markup--h4-anchor" href="http://www.parse.com/products/core" target="_blank" rel="nofollow" data-href="http://www.parse.com/products/core">Core</a>: <strong class="markup--strong markup--p-strong">Store data</strong> and <strong class="markup--strong markup--p-strong">run code</strong> on a remote server.
</p>

<p class="graf--h4" data-align="center">
  <a class="markup--anchor markup--h4-anchor" href="http://parse.com/products/push" target="_blank" rel="nofollow" data-href="http://parse.com/products/push">Push</a>: <strong class="markup--strong markup--p-strong">Send push notifications</strong> to your users via an API or the web composer.
</p>

<p class="graf--h4" data-align="center">
  <a id="edc7"></a><a class="markup--anchor markup--h4-anchor" href="http://parse.com/products/analytics" target="_blank" rel="nofollow" data-href="http://parse.com/products/analytics">Analytics</a>: <strong class="markup--strong markup--p-strong">Track events</strong> in your app and gain valuable insights.
</p>

<p class="graf--p">
  It’s important to note that Parse is built for <strong class="markup--strong markup--p-strong"><em class="markup--em markup--p-em">real, production-level apps</em></strong> — so it has a lot of power under the hood. Thankfully, it also has a simple and easy-to-use API and a JavaScript SDK, so it’s the perfect addition to Framer prototypes. With Parse + Framer, you’ll be able to do almost anything: read and save data, create users and let them log in, remember app states, track events, and so much more.
</p>

<p class="graf--p">
  <a id="6c28"></a>Seriously, look at the <a class="markup--anchor markup--p-anchor" href="http://parse.com/docs" target="_blank" rel="nofollow" data-href="http://parse.com/docs">documentation</a> — you can do pretty much anything.
</p>

* * *

<h2 class="graf--h4" data-align="center">
  Let's get started
</h2>

<h1 class="graf--h3" data-align="center">
   <img class="alignleft wp-image-2663 size-large" src="{{ site.url }}/assets/wp-content/uploads/2014/12/Screen-Shot-2014-12-16-at-9.32.38-AM-1024x883.png" alt="Screen Shot 2014-12-16 at 9.32.38 AM" width="584" height="503" />
</h1>

<h4 class="graf--h4" data-align="center">
  Get an account
</h4>

<p class="graf--p is-withNotes">
  <a id="4ae4"></a>In order to use Parse, you’ll need an account. Head over to <a class="markup--anchor markup--p-anchor" href="http://www.parse.com/" target="_blank" rel="nofollow" data-href="http://www.parse.com">parse.com</a> and click the “<strong class="markup--strong markup--p-strong">Sign up</strong>” button in the top right. Create your account with an email and password, and name your first app (“<em class="markup--em markup--p-em">Framer Prototype</em>” is a good choice, but feel free to name it whatever you like). If you’re just doing simple data storage, you can use the same app for as many prototypes as you want. If you end up using more of what Parse has to offer, you may want to create a new app for each prototype.
</p>

<p class="graf--p is-withNotes">
  <img class="alignleft size-full wp-image-2664" src="{{ site.url }}/assets/wp-content/uploads/2014/12/Screen-Shot-2014-12-15-at-3.18.58-PM.png" alt="Screen Shot 2014-12-15 at 3.18.58 PM" width="583" height="338" />
</p>

<h4 class="graf--h4" data-align="center">
  Grab your keys
</h4>

<p class="graf--p graf--last">
  <a id="2d16"></a>From your <a class="markup--anchor markup--p-anchor" href="http://www.parse.com/apps" target="_blank" rel="nofollow" data-href="http://www.parse.com/apps">Parse dashboard</a>, hover over the gear icon and click keys. We’ll only be using the <strong class="markup--strong markup--p-strong">Application ID</strong> and the <strong class="markup--strong markup--p-strong">JavaScript Key</strong> so feel free to copy them somewhere easy to get to (like an empty text file) or just leave this page open in another tab. Keys are unique strings of characters that act as your password when communicating with Parse. It tells Parse what app you’re trying to connect to, and proves that you are allowed to make that connection.
</p>

<h4 class="graf--h4" data-align="center">
  Plan your data
</h4>

<p class="graf--p is-withNotes">
  <a id="977c"></a>We’ll need some data to pull in to our prototype. Looking at the designs, we’ll probably need a city, a state, a like count, and an image URL. It also might be nice to have a way to toggle certain locations on and off. It’s important to think through this as best as you can before you start prototyping to prevent any messy data issues.
</p>

<p class="graf--p">
  <a id="e5f6"></a>There are a few different special types of data that you can store in Parse. It’s important to make sure you choose the right one for each field. Here are the most common data types you might use in prototyping:
</p>

<p class="graf--p">
  <a id="cf4f"></a><strong class="markup--strong markup--p-strong">String:<br /> </strong>Any kind of text. Whatever you put here is exactly what you’ll get back. We’ll use this type for the city, state and image URL.
</p>

<p class="graf--p">
  <a id="e2cb"></a><strong class="markup--strong markup--p-strong">Number:<br /> </strong>This type is specific for numbers. Using this allows you to do math operations with out any kind of type conversion. We’ll use this type to track the like count in our prototype.
</p>

<p class="graf--p graf--last">
  <a id="bd19"></a><strong class="markup--strong markup--p-strong">Boolean:<br /> </strong>This can be either true or false. It’s helpful for toggles and states. We’ll use this to determine if a location is shown or hidden.
</p>

<h4 class="graf--h4" data-align="center">
  Create your class
</h4>

<p class="graf--p">
  <a id="16ca"></a>In Parse, a set of data is called a <strong class="markup--strong markup--p-strong">class</strong>. This is very similar to a <strong class="markup--strong markup--p-strong">sheet</strong> in your favorite spreadsheet app. From your <a class="markup--anchor markup--p-anchor" href="http://parse.com/apps" target="_blank" rel="nofollow" data-href="http://parse.com/apps">dashboard</a>, hover over the app and click on “Core” at the bottom. This will take you to the Parse data browser.
</p>

<p class="graf--p">
  <a id="31c8"></a>Here, we’re going to create a new class called “Locations” and add columns for the different types of data that we planned for. Once you’ve created the class, click the “<strong class="markup--strong markup--p-strong">+ Col</strong>” button to add a new column (remember to use the right types that we defined in the previous step). Once you’ve made all the right columns, go ahead and add some rows of data. Here’s a screenshot of the complete class I made for this example:
</p>

<p class="graf--p">
  <img class="alignleft wp-image-2665" src="{{ site.url }}/assets/wp-content/uploads/2014/12/Screen-Shot-2014-12-15-at-3.44.44-PM.png" alt="" width="900" height="315" />
</p>

<div class="section-inner sectionLayout--outsetColumn">
  <p class="graf--figure postField--outsetCenterImage">
    Note: Column names are case-sensitive, so be sure you are consistent. I prefer to use camel case (i.e. likeCount vs LikeCount) and that is how the code in this guide is written.
  </p>
</div>

<div class="section-inner layoutSingleColumn">
  <p class="graf--p">
    <a id="7f2a"></a>You can see the data is pretty straight forward. If you’ve ever used any kind of spreadsheet application, this will feel very familiar. The only columns you really need to worry about with prototypes are the ones you’ve created. You can safely ignore all the Parse columns (<em class="markup--em markup--p-em">like objectId, createdAt, etc.</em>).
  </p>
  
  <p class="graf--p graf--last">
    <a id="da44"></a>For the images, all I’ve done is dropped in a direct link to some photos from Instagram. If you want to use your own images, you can use something like<a class="markup--anchor markup--p-anchor" href="http://imgur.com/" target="_blank" rel="nofollow" data-href="http://imgur.com">Imgur</a> or even just a public Dropbox folder. I’ve also preset a couple like counts, but you could leave these as all zeroes too.
  </p>
  
  <p class="graf--p graf--last">
    <img class="alignleft size-large wp-image-2666" src="{{ site.url }}/assets/wp-content/uploads/2014/12/Artboard-1-1024x411.jpg" alt="" width="584" height="234" />
  </p>
  
  <p class="graf--p" data-align="center">
    <em class="markup--em markup--p-em">(this is where the magic happens)</em>
  </p>
  
  <h4 class="graf--h4" data-align="center">
    <a id="039e"></a>Integrate with Framer
  </h4>
  
  <blockquote class="graf--blockquote">
    <p>
      <a id="a0f1"></a>If you’re in a rush and want to save some time you can download this blank <a class="markup--anchor markup--blockquote-anchor" href="http://framer.link/cl.ly/1R1S0u2A0505/download/Blank%20Parse%20Template.framer.zip" target="_blank" rel="nofollow" data-href="http://framer.link/cl.ly/1R1S0u2A0505/download/Blank%20Parse%20Template.framer.zip">Parse + Framer template</a> that has the Parse SDK pre-installed and skip to the next step.
    </p>
  </blockquote>
  
  <p class="graf--p is-withNotes">
    <a id="bfbf"></a>Now that we’ve got a class and some data, we can finally start our Framer integration! Luckily, this part is super easy. Start a new <strong class="markup--strong markup--p-strong">Framer Studio </strong>project and paste in this code:
  </p>
  
  <pre class="line-numbers"><code class="language-coffeescript"># Initialize Parse with the app keys corresponding to your app
Parse.initialize("APPLICATION ID", "JAVASCRIPT KEY");

# Write your Framer Code below this
success = new BackgroundLayer backgroundColor: 'green'</code></pre>
  
  <p>
    Now you’re probably getting an error in Framer Studio, which is OK. This is because Framer doesn’t know what Parse is yet. We have to add the Parse JavaScript SDK to the index file for your Framer project.   <a id="82db"></a>Save your project and close it. Open up Finder, navigate to where your project is, and click on it. You should see something like this:       Open up the “index.html” file in your favorite code editor (I prefer<a class="markup--anchor markup--p-anchor" href="http://brackets.io/" target="_blank" rel="nofollow" data-href="http://brackets.io">Brackets</a>) and add this line as the first script in the script block:
  </p>
  
  <pre class="line-numbers"><code class="language-markup">&lt;script src=”https://www.parsecdn.com/js/parse-1.3.2.min.js"&gt;&lt;/script&gt;</code></pre>
  
  <p>
    <a id="90cc"></a>Your index file should look like this now:
  </p>
  
  <pre class="line-numbers"><code class="language-markup">&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;

&lt;meta name="format-detection" content="telephone=no"&gt;
&lt;meta name="apple-mobile-web-app-capable" content="yes"&gt;
&lt;meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"&gt;
&lt;meta name="viewport" content="width=device-width, height=device-height, initial-scale=0.5, maximum-scale=0.5, user-scalable=no"&gt;

&lt;link rel="apple-touch-icon" href="framer/images/icon-120.png"&gt;
&lt;link rel="apple-touch-icon" href="framer/images/icon-76.png" sizes="76x76"&gt;
&lt;link rel="apple-touch-icon" href="framer/images/icon-120.png" sizes="120x120"&gt;
&lt;link rel="apple-touch-icon" href="framer/images/icon-152.png" sizes="152x152"&gt;

&lt;link rel="stylesheet" type="text/css" href="framer/style.css"&gt;

&lt;script src="https://www.parsecdn.com/js/parse-1.3.2.min.js"&gt;&lt;/script&gt;
&lt;script src="framer/coffee-script.js"&gt;&lt;/script&gt;
&lt;script src="framer/framer.js"&gt;&lt;/script&gt;
&lt;script src="framer/framer.generated.js"&gt;&lt;/script&gt;
&lt;script src="framer/init.js"&gt;&lt;/script&gt;

&lt;/head&gt;
&lt;body&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
  
  <p>
    <span style="line-height: 1.5;"> </span>Once you’ve added the line, save the file and close it. Now re-open your project in Framer Studio, and your device should have a green screen. Congrats, you’ve successfully initialized your Parse app!
  </p>
  
  <h4 class="graf--h4 graf--first" data-align="center">
    Write a query
  </h4>
  
  <p class="graf--p">
    <a id="0839"></a>Everything in Parse that relates to reading data is centered around the <a class="markup--anchor markup--p-anchor" href="https://parse.com/docs/js_guide#queries" target="_blank" rel="nofollow" data-href="https://parse.com/docs/js_guide#queries"><strong class="markup--strong markup--p-strong">query</strong></a>. Luckily, queries are pretty easy to write and it’s a piece of cake to sift through the data you get back. Let’s get our list of locations from the class we made earlier:
  </p>
  
  <pre class="line-numbers"><code class="language-coffeescript"># Initialize Parse with the app keys corresponding to your app
Parse.initialize("APPLICATION ID", "JAVASCRIPT KEY");

# create an object that extends the class you want to pull data from
Locations = Parse.Object.extend("Locations")

# create a query against the object you just created
queryLocations = new Parse.Query(Locations)

# we're looking for rows that have show set to true
queryLocations.equalTo "show", true

# we're going to sort the response by state and then by city
queryLocations.ascending "state", "city"

# now we actually run the query and get back results
queryLocations.find
success: (results) -&gt;
# if the query is a success, we'll get back a results object.
# this for loop lets us do something to each result in that larger "results" object
for result, i in results
location = new Layer width: 750, height: 300, y: 300 * i
location.html = result.attributes.city + ", " + result.attributes.state
location.image = result.attributes.image
location.name = result.id
# ^^^ super important! the result.id is the ID that Parse
# uses to find that entry. we need to associate it with this
# layer if we ever want to interact with this data in the future
error: (error) -&gt;
# if there was an error, we'd print it
print error
</code></pre>
  
  <p>
    You can see on lines 23–25 how we access data thats returned from Parse. We get a big “<code>results</code>” object back that we then loop through and act on each “<code>result</code>.” Each column in your class is exposed as an attribute of a result, so for example if you had a column called <code>“cuteKittens”</code> you would get to it by looking at <code>result.attributes.cuteKittens</code>.
  </p>
  
  <p class="graf--p">
    If you’ve followed along correctly so far, you should end up with something like this.
  </p>
  
  <p class="graf--p">
    <img class="alignleft size-large wp-image-2667" src="{{ site.url }}/assets/wp-content/uploads/2014/12/Screen-Shot-2014-12-15-at-4.16.15-PM-631x1024.png" alt="" width="584" height="947" />
  </p>
  
  <p class="graf--p">
    Obviously we have a lot of styling and overall prettifying work to do, but hey you just pulled some dynamic data in to your prototype!
  </p>
  
  <h1 class="graf--h3 is-withNotes">
    <a id="ebc0"></a><strong class="markup--strong markup--h3-strong">High five!</strong>
  </h1>
  
  <h4 class="graf--h4 graf--first" data-align="center">
    Write some data back
  </h4>
  
  <p class="graf--p">
    <a id="3eb6"></a>Now that we’re able to read data in, we might want to save something back to our database. For this example, we’re going to create a like function that increments the like count in our app and save it back to Parse.
  </p>
  
  <pre class="line-numbers"><code class="language-coffeescript"># Initialize Parse with the app keys corresponding to your app
Parse.initialize("APPLICATION ID", "JAVASCRIPT KEY");

# create an object that extends the class you want to pull data from
Locations = Parse.Object.extend("Locations")

# create a query against the object you just created
queryLocations = new Parse.Query(Locations)

# we're looking for rows that have show set to true
queryLocations.equalTo "show", true

# we're going to sort the response by state and then by city
queryLocations.ascending "state", "city"

# now we actually run the query and get back results
queryLocations.find
success: (results) -&gt;
# if the query is a success, we'll get back a results object.
# this for loop lets us do something to each result in that larger "results" object
for result, i in results
location = new Layer width: 750, height: 300, y: 300 * i
location.html = result.attributes.city + ", " + result.attributes.state
location.image = result.attributes.image
location.name = result.id
# ^^^ super important! the result.id is the ID that Parse
# uses to find that entry. we need to associate it with this
# layer if we ever want to interact with this data in the future

location.on Events.Click, -&gt;
# same as when we initially pulled the location list,
# we need to retrieve the item we want to increment from Parse
# so we will need to query our Locations class
likeGet = new Parse.Query(Locations)

# the @ symbol is coffeescript's equivalent of "this"
# so here we're telling our new Parse.Query to fetch the
# row that corresponds with the ID we set back up on line 25
likeGet.get @.name,
# if our call is successful, we'll get back a single row which I've named 'location'
success: (location) -&gt;
# since likes is the number type we can
# use this handy Parse helper called increment
# which will take the specific column 'likes' of
# our 'location' and increment it by 1
location.increment('likes')

# and now all we have to do is call
# save() on our location and the new like count is stored
location.save()

error: (error) -&gt;
# if there was an error, we'd print it
print error
</code></pre>
  
  <p>
    Now nothing will happen visually in our app because we’re not actually displaying the likes anywhere yet, but if you refresh your Parse data browser you’ll see the like counts going up by one every time you click a location. You’re now reading and writing to a cloud database, all from inside your own Framer prototype!
  </p>
  
  <h4 class="graf--h4" data-align="center">
    Keep going
  </h4>
  
  <p class="graf--p is-withNotes">
    <a id="bb0f"></a>By now, you should have a semi-functional prototype and basic mental idea of how Parse works. I’ve completely built out this example prototype below, so download it and take a look at how it works. Also, there is a <a class="markup--anchor markup--p-anchor" href="https://parse.com/docs/js_guide" target="_blank" rel="nofollow" data-href="https://parse.com/docs/js_guide">ton of documentation</a> on Parse that can teach you how to take this example even further. Take what you’ve experimented with here and use that knowledge to enhance your own prototypes!
  </p>
  
  <hr />
  <section> 
  
  <div class="section-content">
    <div class="section-inner layoutSingleColumn">
      <h1 class="graf--h3 graf--first" data-align="center">
        Explore the finished project
      </h1>
      
      <p class="graf--p graf--last" data-align="center">
        <a id="3f0b"></a><a class="markup--anchor markup--p-anchor" href="http://framer.link/cl.ly/0q0C1A1B2y07/download/Parse_Data_FINAL.framer.zip" target="_blank" rel="nofollow" data-href="http://framer.link/cl.ly/0q0C1A1B2y07/download/Parse_Data_FINAL.framer.zip"><strong class="markup--strong markup--p-strong">Check it out in Framer Studio</strong></a> <em class="markup--em markup--p-em">or</em> <a class="markup--anchor markup--p-anchor" href="http://cl.ly/0q0C1A1B2y07" target="_blank" rel="nofollow" data-href="http://cl.ly/0q0C1A1B2y07"><strong class="markup--strong markup--p-strong">download the source</strong></a><strong class="markup--strong markup--p-strong">.</strong>
      </p>
    </div>
  </div></section> <section class=" section--last"> 
  
  <div class="section-content">
    <div class="section-inner layoutSingleColumn">
      <blockquote>
        <p>
          <a id="7724"></a>Thanks for reading! Please give me a <a class="markup--anchor markup--blockquote-anchor" href="http://twitter.com/gk3" target="_blank" rel="nofollow" data-href="http://twitter.com/gk3">shout on Twitter</a> if you have any questions or if you have ideas for future guides you’d like to see.
        </p>
      </blockquote>
    </div>
  </div></section>
</div>