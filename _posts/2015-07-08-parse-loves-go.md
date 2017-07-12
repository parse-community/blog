---
id: 3636
title: Parse Loves Go
date: 2015-07-08T08:25:57+00:00
author: Abhishek Kona
layout: post
guid: http://blog.parse.com/?p=3636
permalink: /learn/parse-loves-go/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3916160899"
categories:
  - Engineering
  - Learn
tags:
  - go
  - libraries
---
Recently, we shared our story on [moving our stack from Ruby to Go](http://blog.parse.com/learn/how-we-moved-our-api-from-ruby-to-go-and-saved-our-sanity/) over the course of the last two years. This week, a few of us are attending [Gophercon 2015](http://www.gophercon.com/) to bond with our fellow gophers. In this post you'll find a quick roundup with details on a few of the libraries and tools we've built in Go. Join us on Wednesday at 2:30 PM for our [talk](http://www.gophercon.com/talks/rebuilding-parse/) on rewriting Parse in Go!

<ul class="standard-list">
  <li>
    We use <b>Inject</b>,<b> </b>a dependency injection library, to build our dependency graph. We previously <a href="http://blog.parse.com/learn/engineering/dependency-injection-with-go/">blogged about Inject here</a>.
  </li>
  <li>
    We built a connection pooling proxy for MongoDB called <b>Dvara</b>, which also instruments counters and metrics around every DB operation. This has been battle tested against both the Ruby and Go Mongo drivers in production. Find out more about<a href="http://blog.parse.com/learn/engineering/dvara/"> Dvara here</a>.
  </li>
  <li>
    We built the library <a href="https://github.com/facebookgo/grace"><b>Grace</b></a> to gracefully restart servers. Grace hands down the socket from the old process to the new, and lets the old process die off gracefully without dropping a single request.
  </li>
  <li>
    <a href="http://github.com/facebookgo/stackerr"><b>Stackerr</b></a> wraps all our errors with a stack trace and helps us track down the root cause of exceptions.
  </li>
  <li>
    We use <a href="http://github.com/facebookgo/muster"><b>Muster</b></a> to fire our requests in a batch based on time or queue size.
  </li>
  <li>
    <a href="http://github.com/facebookgo/startstop"><b>Start-Stop</b> </a>is a library to start and stop components in the order of dependency. This helps us not write cumbersome start stop logic in each and every package.
  </li>
  <li>
    We also have several other libraries including<b> <a href="http://github.com/facebookgo/httpcontrol">httpcontrol</a>, <a href="http://github.com/facebookgo/ensure">ensure</a>, </b>and more.
  </li>
</ul>

Check out our [GitHub repo](http://github.com/facebookgo/) for other libraries. We hope you can use these in your very own Go projects!