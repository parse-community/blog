---
id: 3964
title: 'Open Source Spotlight: Florent Vilmart'
date: 2016-03-14T14:30:34+00:00
author: foscomarotto
layout: post
guid: http://blog.parseplatform.org/?p=3964
permalink: /learn/open-source-spotlight-florent-vilmart/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Engineering
  - Learn
---
Here at Parse, we are big fans of open source software and the communities which form to build things together. [Parse Server](https://github.com/parseplatform/parse-server), released just 47 days ago, has already seen 50 contributors join the team. It's incredibly exciting to see this project moving so quickly, with new features being added along with many improvements and fixes. We've been open-minded to suggestions, and have favored accepting contributions over enforcing too many rules. The Parse eco-system is yours now, and it's very important that you feel some ownership.

One outstanding contributor, [Florent Vilmart](https://twitter.com/flovilmart), has been [very involved](https://github.com/ParsePlatform/parse-server/issues?utf8=%E2%9C%93&q=mentions%3Aflovilmart) in both high-level design discussions and contributed a lot of code. We wanted to learn more about his thoughts towards Parse and open-source software in general, and he agreed to take part in the following Q&A:

* * *

## Q: How did you first get started with Parse?

<ol class="standard-list">
  <p>
    <quote>I heard about Parse as soon as it officially launched. We were building an app at the time, and despite my knowledge in backends, Parse looked like the ideal prototyping solution. Without the backend in the way, we could focus on getting the iOS app out and iterate from there. Since then, it’s been my first choice for proof-of-concept apps, MVPs and experiments.</quote>
  </p>
</ol>

## Q: Why do you like developing with Parse?

<ol class="standard-list">
 <p><quote>It’s just so fast! Think about it for a minute, you can get a data store (with proper management of access rights for your users!), social login and push notifications in just a few clicks. How long would it take you to develop that yourself? days, maybe weeks! It’s not without constraints obviously, but with the open-source announcement, those barriers are about to be removed!</quote></p>
  
  <p><quote>I recently built a project for a client which required video transcoding, a complex management interface with different levels of roles, and a native iOS app. Parse was a key element for the success of that project. The ACLs made it easy for the content contributors to safely input metadata to the platform, as well as keeping user generated content private. The management interface was built with the awesome parse-react. The iOS app made heavy usage of the local data store. Parse integrated perfectly with the complex production environment, python servers for video processing, node.js front-ends, web sockets, AWS queues and S3 events were all part of the architecture.</quote></p>
</ol>

## Q: Where do you think Parse Server is headed?
    
<ol class="standard-list">
    <p><quote>Since 2011, we have seen the rise of and a consolidation of the BaaS offerings. The market has matured, and many options are available to developers. Parse Server is uniquely positioned in that landscape as the developer is now in control of its datastore, and as the project matures we’ll see more stability and incredible features pushed by the community. I truly think it’s great that Facebook open sourced Parse Server, and the migration timeline is more than reasonable.</quote></p>
    <blockquote>
      <p>
        I can feel the motivation from the Parse team to build an amazing open source project.
      </p>
    </blockquote>
    <p><quote>All the SDK’s have been open sourced, and all of them leverage very smart architectures, that have been proven over the years, combining this difficult balance of power and ease of use. Parse Server is headed that way too. Yes, the birth was painful, and many bugs remain to squash, but looking at the last 2 change logs you can feel this highly motivated community wanting to push it to the next level.</quote></p>
</ol>
        
## Q: What does open-source mean to you?
        
<ol class="standard-list">
  <p>
    <quote>I’ve had many roles over the years, from junior iOS developer to CTO with a team of 10 people, and I’ve always consumed open source projects. They make your life so much easier when you start coding, or when you need to ship a product quickly. Over the years, I managed to get a bit better, so I started to contribute back. In a certain way, that’s my way of saying thank you for all those projects that I used and saved me hours of sleep.</quote>
  </p>

  <p>
    <quote>Now, open source is a way to work with talented people from across the world. The PR review process is probably the part that I like the most. Getting shut down by a Facebook engineer that is probably 5-10 years younger than me is invaluable. You get to learn so much about your code and about you. It allows you to see new ways of solving the same problems which in turn makes you a better developer. It’s one of the best schools to stay on top of your game. At the end of the day, the most important is to be happy and proud of the work you’re doing.</quote>
  </p>
</ol>
            
## Q: How would you advise others on becoming open-source contributors?
            
<ol class="standard-list">
  <p><quote>First, you don’t have to be a rock star to contribute.</quote></p>

  <p>
    <quote>Contributions start with reporting issues. Without issues reported by the community, the core team can’t do much. That being said, reporting an issue is maybe the most difficult part of the contribution. You are noticing something weird happening, now it’s time to check your code, and see if you’re doing something wrong before blaming the tool. Then you try to get to the bottom of it, pin-point areas where it works and where it doesn’t. It takes some time, but that will make your contributions better, and you’ll have a better chance for it to be solved quickly! Imagine that we don’t know anything about the project, and you have to explain exactly what the problem is, that leads me to the next phase.</quote>
  </p>

  <blockquote>
    <p>
      There is no better way to explain a problem, than a failing unit test!
    </p>
  </blockquote>

  <p>
    <quote>Open-source heavily relies on unit tests, they are the common source of truth. They describe how things should be and not be. Writing failing tests is usually easy, just make sure that the test reproduces your use case and that the ‘working’ cases are covered as well. It’s better to have multiple tests that cover the same scenario, than none.</quote>
  </p>

  <p>
    <quote>Continuing from the previous step of creating a failing test, the next way to contribute is to open a pull request. You don’t have to rush to the solution; take your time, ask questions, propose solutions. It’s team work, and the reviewer will help you along the way. When a reviewer is not happy about your contribution, or seems a bit blunt, don’t take it badly, that’s normal. We see a lot of code going through, and conciseness is key for our mental health! And remember that sometimes, reviewers are wrong... We all make errors, we all forget about things or sometimes don’t pay attention enough. Remember it’s a team effort, take a deep breath, and jump in!</quote>
  </p>

  <blockquote>
    <p>
      Remember it’s a team effort, take a deep breath, and jump in!
    </p>
  </blockquote>
</ol>

<hr />

<p>
  Our thanks to Florent, and all of the open source community, for their amazing contributions! Happy Building!
</p>
