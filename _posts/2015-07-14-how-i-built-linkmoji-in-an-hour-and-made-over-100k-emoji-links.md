---
id: 3640
title: How I Built Linkmoji in an Hour (and made over 100k emoji links)
date: 2015-07-14T08:58:21+00:00
author: ericnakagawa
layout: post
guid: http://blog.parseplatform.org/?p=3640
permalink: /learn/how-i-built-linkmoji-in-an-hour-and-made-over-100k-emoji-links/
post_format:
  - feat-image
feature_image_style:
  - large
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3933999297"
image: /wp-content/uploads/2015/07/linkmoji-hero.png
categories:
  - Learn
  - Tutorial
tags:
  - core
---
<section class=" section--body"> 

<p class="section-divider layoutSingleColumn">
  <em>This is a personal story written by Eric Nakagawa, Parse Developer Advocate, about his side-project <a href="http://www.xn--vi8hiv.ws/">linkmoji</a>. linkmoji is built on Parse. linkmoji is hosted on the free tier and currently uses less than half the 30 req/s available! This story was originally published on <a href="https://medium.com/@ericnakagawa/how-i-built-linkmoji-in-an-hour-fab662e37960">Medium</a>.</em>
</p></section> <section class=" section--body"> 

<div class="section-content">
  <div class="section-inner layoutSingleColumn">
    <hr />
    
    <p class="graf--p">
      While hanging out at my neighborhood public library this week, I built <a class="markup--anchor markup--p-anchor" href="http://www.xn--vi8hiv.ws/">linkmoji</a>, a link shortener that lets you easily convert a long URL into a short URL that’s made up of emoji! I created it with my friend and fellow emoji-fanatic <a class="markup--anchor markup--p-anchor" href="http://🍕💩.ws/💯💯💯">George</a> on Wednesday night. In the last 36 hours, we’ve currently seen over <strong class="markup--strong markup--p-strong">100,000</strong> emoji links created!!
    </p>
  </div>
  
  <div class="section-inner sectionLayout--outsetColumn">
    <figure class="graf--figure graf--layoutOutsetCenter">
      <div class="aspectRatioPlaceholder is-locked">
        <p>
          <img class=" aligncenter" src="https://d262ilb51hltx0.cloudfront.net/max/2000/1*ZnrLZ4wH0nBAlGptqIERlA.png" alt="" width="998" height="173" />
        </p>
      </div>
      </figure>
  </div>
  
  <div class="section-inner layoutSingleColumn">
    <h4 class="graf--h4">
      Ok, but why?
    </h4>
    <p class="graf--p">
      A <a class="markup--anchor markup--p-anchor" href="http://forge.im/jared">friend</a> challenged me to build a link shortener over the weekend. Initially I set out to build a link shortener that would a generate a random selection of words. ex. <a class="markup--anchor markup--p-anchor" href="http://🍕💩.ws/pizza-king">http://🍕💩.ws/pizza-king</a> — but while writing the logic for selecting words from a dictionary, I realized it would be more fun to use emoji.
    </p>
  </div>
</div></section> <section class=" section--body"> 

<div class="section-divider layoutSingleColumn">
  <hr class="section-divider" />
</div>

<div class="section-content">
  <div class="section-inner layoutSingleColumn">
    <h4 class="graf--h4">
      Building linkmoji
    </h4>
    <p class="graf--p">
      I grabbed a bunch of my favorite emoji, put them into a comma separated array, and started hacking away at a web app.
    </p>
    <p>
      Next I built a function that recursively iterates and generates a string of emoji. At our current count we have 100+ emoji supported. The algorithm selects one of the 100, removes that emoji from the list, then randomly selects from the remaining emoji until the total number of emoji characters is reached. (100 x 99 x 98 x 99 x 98 x 97 x 96 x 95 = 8,327,010,517,056,000 (8 quadrillon 😱) potential linkmojis. It is quite doubtful we’ll ever use up all possible combinations, but in the meantime I’ll keep adding more emoji to the dictionary. Once a string of emoji is generated, it is saved to Parse. I set up a beforeSave function to query the links table to check if any other link has the same emoji and to generate a new one if it already exists. This should prevent emoji collisions.   The way I generate textmoji is by first creating a JSON object that maps emoji to English words. I then convert the emoji string into an array, iterate through, and build textmoji strings (one with, and one without hyphens). Here is the map of emoji to text.
    </p>
    
    <p class="graf--p">
      I uncovered several issues with how Javascript handles unicode. Since unicode uses more space than a normal string character, it requires different handling. The current implementation of JS doesn’t support easily iterating through strings with unicode, but I found <a class="markup--anchor markup--p-anchor" href="http://stackoverflow.com/questions/24531751/how-can-i-split-a-string-containing-emoji-into-an-array/24531752#24531752">this function</a> that converts the unicode string into an Array. An array is exactly what I needed for generating these strings as I use each emoji as a key when looking up the corresponding text value for each mapped object. (👑 -> “crown”).
    </p>
    
    <p class="graf--p">
      From here it was all a matter of displaying the information after the user had submitted their links. This is done by rendering an html file converted to ejs, passing the linkmoji and txtmoji urls to the template, rendering and sending to the browser (all handled by ExpressJS and Parse Webhosting).
    </p>
    
    <h4 class="graf--h4">
      Visiting a linkmoji
    </h4>
    
    <p class="graf--p">
      Because I don’t know whether a visitor is providing a linkmoji, textmoji, or textmoji without hyphens, I OR together multiple several Parse queries and take the first, oldest link by create date. I also update the view count and then route the user. Here’s the code:
    </p>
    
    <p>
    </p>
  </div>
</div></section> <section class=" section--body"> 

<div class="section-content">
  <div class="section-inner layoutSingleColumn">
    <h3 class="graf--h3">
      The Launch
    </h3>
    
    <p class="graf--p">
      Around 6pm Wednesday I tweeted out a very roughly hacked together website.
    </p>
    
    <blockquote class="twitter-tweet" width="550">
      <p lang="en" dir="ltr">
        I built this today: <a href="http://t.co/zwfUBBiyCw">http://t.co/zwfUBBiyCw</a>🍕💩&#10;&#10;I call it linkmoji.
      </p>
      
      <p>
        &mdash; Eric Nakagawa (ナカガワ) (@ericnakagawa) <a href="https://twitter.com/ericnakagawa/status/618950426369593344">July 9, 2015</a>
      </p>
    </blockquote>
    
    <p>
    </p>
    
    <p class="graf--p">
      <a class="markup--anchor markup--p-anchor" href="http://twitter.com/gk3">George</a> sees my tweet and sends me a message offering to do give it a real design. I happily accept.
    </p>
    
    <p class="graf--p">
      <em class="markup--em markup--p-em">Disclaimer: George and </em><a class="markup--anchor markup--p-anchor" href="http://eric@parse.com"><em class="markup--em markup--p-em">Eric</em></a><em class="markup--em markup--p-em"> both work on the Parse team (acquired by Facebook </em>👍<em class="markup--em markup--p-em">). Linkmoji is a personal project they build on their own time.</em>
    </p>
    
    <p class="graf--p">
      An hour later someone from twitter suggests I post on Product Hunt.
    </p>
    
    <blockquote class="twitter-tweet" width="550">
      <p lang="en" dir="ltr">
        <a href="https://twitter.com/ericnakagawa">@ericnakagawa</a> You should post on <a href="https://twitter.com/ProductHunt">@ProductHunt</a> I bet <a href="https://twitter.com/rrhoover">@rrhoover</a> would love this
      </p>
      
      <p>
        &mdash; Andrew So (@AndrewDixonSo) <a href="https://twitter.com/AndrewDixonSo/status/618968054186119169">July 9, 2015</a>
      </p>
    </blockquote>
    
    <p>
    </p>
    
    <p>
      I make put together a funny cat + glasses linkmoji for ProductHunt and tag Ryan Hoover.
    </p>
    
    <blockquote class="twitter-tweet" width="550">
      <p lang="en" dir="ltr">
        <a href="https://twitter.com/AndrewDixonSo">@AndrewDixonSo</a> <a href="https://twitter.com/ProductHunt">@ProductHunt</a> <a href="https://twitter.com/rrhoover">@rrhoover</a> I made this for Product Hunt <a href="http://t.co/zwfUBBiyCw">http://t.co/zwfUBBiyCw</a>😸👓 - an homage to Google Glass Kitten!
      </p>
      
      <p>
        &mdash; Eric Nakagawa (ナカガワ) (@ericnakagawa) <a href="https://twitter.com/ericnakagawa/status/618971714848993280">July 9, 2015</a>
      </p>
    </blockquote>
    
    <p>
    </p>
    
    <p class="graf--p">
      I wasn’t going to post to PH until I saw the CEO cheer me on.
    </p>
    
    <blockquote class="twitter-tweet" width="550">
      <p lang="en" dir="ltr">
        <a href="https://twitter.com/ericnakagawa">@ericnakagawa</a> <3! You should post it
      </p>
      
      <p>
        &mdash; Ryan Hoover (@rrhoover) <a href="https://twitter.com/rrhoover/status/618982228194758656">July 9, 2015</a>
      </p>
    </blockquote>
    
    <p>
    </p>
    
    <p class="graf--p">
      We spend the next few hours adding features and tightening up the design and head.
    </p>
    
    <p class="graf--p">
      I head to bed around midnight.
    </p><figure class="graf--figure"> 
    
    <div class="aspectRatioPlaceholder is-locked">
      <div class="aspect-ratio-fill">
      </div>
      
      <div style="text-align: center;">
        <img class="graf-image alignnone" src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*Buw95D0MJ3EeQRljZ2rGww.png" alt="" width="462" height="76" />
      </div>
    </div></figure> 
    
    <h3 class="graf--h3">
      OMG!
    </h3>
    
    <p class="graf--p">
      I wake up to this message…
    </p><figure class="graf--figure"> 
    
    <div class="aspectRatioPlaceholder is-locked">
      <div class="aspect-ratio-fill">
      </div>
      
      <div style="text-align: center;">
        <img class="graf-image alignnone" src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*u_L6xdBMUwCk0ASM1WqZBQ.png" alt="" width="462" height="69" />
      </div>
    </div></figure> 
    
    <p class="graf--p">
      I jump on to see how many people are on the website and…
    </p><figure id="attachment_3645" style="width: 1722px" class="wp-caption alignnone">
    
    <img class="size-full wp-image-3645" src="{{ site.url }}/assets/wp-content/uploads/2015/07/wat.jpg" alt="whoa" width="1722" height="682" srcset="{{ site.url }}/assets/wp-content/uploads/2015/07/wat.jpg 1722w, {{ site.url }}/assets/wp-content/uploads/2015/07/wat-300x119.jpg 300w, {{ site.url }}/assets/wp-content/uploads/2015/07/wat-1024x406.jpg 1024w, {{ site.url }}/assets/wp-content/uploads/2015/07/wat-875x347.jpg 875w" sizes="(max-width: 1722px) 100vw, 1722px" /><figcaption class="wp-caption-text">whoa</figcaption></figure> <figure class="graf--figure graf--startsWithDoubleQuote"> 
    
    <div class="aspectRatioPlaceholder is-locked">
    </div></figure> <figure class="graf--figure graf--startsWithDoubleQuote"> 
    
    <div class="aspectRatioPlaceholder is-locked">
    </div>
    
    <h3 style="text-align: center;">
      The rest of the day was crazy!
    </h3></figure> 
    
    <p class="graf--p">
      People begin linking to us from everywhere!
    </p>
    
    <ul class="standard-list">
      <li>
        <a href="http://masatolan.com/Webservice/linkmoji/">A fan guide from Japan!</a>
      </li>
      <li>
        <a href="http://mashable.com/2015/07/09/linkmoji-links-emoji/">Mashable</a>
      </li>
      <li>
        <a href="https://www.reddit.com/r/InternetIsBeautiful/comments/3cnvx0/use_ws_to_shorten_your_links_to_emojis/">Reddit</a>
      </li>
      <li>
        <a href="https://news.ycombinator.com/item?id=9858405">HackerNews</a>
      </li>
      <li>
        <a href="http://www.publimetro.com.mx/tecno/asi-seria-la-url-de-sus-paginas-preferidas-de-internet-con-emojis/XeSogi!i5OWXKBeIl5lz_DnVwp8gA/">PubliMetro</a>
      </li>
      <li>
        <a href="http://www.wired.com/2015/07/linkmoji-turns-urls-emoji/">Wired</a>
      </li>
      <li>
        <a href="http://www.cosmopolitan.com/lifestyle/news/a43133/we-may-never-need-words-again-this-site-turns-any-link-into-emoji/">Cosmopolitan</a>
      </li>
      <li>
        <a href="http://www.huffingtonpost.com/2015/07/10/type-links-in-emojis_n_7771190.html">Huffington Post</a>
      </li>
      <li>
        <a href="http://mic.com/articles/122035/linkmoji-web-tool-emoji-links">MIC</a>
      </li>
      <li>
        <a href="http://www.techradar.com/us/news/internet/turn-any-link-into-emoji-1298764">TechRadar</a>
      </li>
      <li>
        <a href="http://www.salon.com/2015/07/10/can_you_write_a_best_seller_using_only_emoji/">Salon</a>
      </li>
    </ul>
    
    <p class="graf--p">
      We end the day #1 on <a class="markup--anchor markup--p-anchor" href="http://www.producthunt.com/tech/linkmoji">Product Hunt</a>!
    </p>
    
    <h3 class="graf--h4">
      🎉Yay!🎉
    </h3>
    
    <p>
      &nbsp;
    </p><figure style="width: 701px" class="wp-caption aligncenter">
    
    <a href="http://www.producthunt.com/tech/linkmoji"><img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*dhSWMNKdX1zjIQim5-tQvA.png" /></a><figcaption class="wp-caption-text">aw yeah</figcaption></figure> 
    
    <h3 class="graf--h3">
      What’s next?
    </h3>
    
    <p class="graf--p">
      More fun features of course!
    </p>
    
    <p class="graf--p">
      The initial version launched with only 6-emoji characters. Since then we’ve:
    </p>
    
    <ul class="standard-list">
      <li>
        Added support for textmoji (converting emoji into text). We found that some linkmoji wasn’t shareable on some social networks. Txtmoji solves this. When you create a linkmoji, you’re given the option to copy the generated link or a text version. <a href="http://pizza-poop.ws">http://pizza-poop.ws</a>
      </li>
      <li>
        Added a SFW version of the link for people that don’t like sharing links with poop in them. <a href="http://🆒🔗.ws">http://🆒🔗.ws</a>
      </li>
      <li>
        And by popular demand, I am adding a way to customize emoji!
      </li>
    </ul>
    
    <h4 class="graf--h4">
      That’s all!
    </h4>
    
    <p class="graf--p">
      Follow <a class="markup--anchor markup--p-anchor" href="http://twitter.com/linkmoji">@linkmoji on Twitter</a>.
    </p>
    
    <p class="graf--p">
      I challenge you to keep building awesome stuff!
    </p>
    
    <p class="graf--p">
      The text all the way at the bottom that only a special few will read…
    </p>
    
    <p class="graf--p">
      Thanks to everyone who read this far, to those who love making linkmoji, shared ideas, previewed this draft, put up with my crazy ideas, and to those who upvoted on Reddit, HackerNews, ProductHunt, <a class="markup--anchor markup--p-anchor" href="https://www.facebook.com/pages/Linkmoji/884664038268209?fref=ts">Facebook</a>, Twitter, and everywhere else! ❤
    </p>
    
    <p class="graf--p">
      - <a class="markup--anchor markup--p-anchor" href="http://🍕💩.ws/🍕👑">Eric 🍕</a>, co-creator of <a class="markup--anchor markup--p-anchor" href="http://linkmoji.co">linkmoji</a> (and <a class="markup--anchor markup--p-anchor" href="http://🍕💩.ws/😸🍔😸🍔😸🍔">other</a> silly internet things)
    </p>
  </div>
</div></section>