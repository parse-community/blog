---
id: 3919
title: Hosting Your Own Parse on AWS and Heroku
date: 2016-02-04T14:00:55+00:00
author: James Yu
layout: post
guid: http://blog.parse.com/?p=3919
permalink: /announcements/hosting-your-own-parse-on-aws-and-heroku/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Announcements
---
Last week, we released [Parse Server](http://blog.parse.com/announcements/introducing-parse-server-and-the-database-migration-tool/), the open source version of the Parse backend. Since then, the repository has already received over 4,000 stars, 700 forks, and dozens of pull requests. We are working hard to make it the best open source app framework. Developers with an existing Parse app should [migrate their apps to Parse Server now](https://www.parse.com/docs/server/guide). In addition, if you’re building a new mobile app, Parse Server is a great way to build out your backend, coupled with the [Parse SDKs](https://parse.com/docs/downloads).

Today, we want to highlight some great options for hosting Parse Server. We partnered closely with [Heroku](https://www.heroku.com), [Amazon Web Services (AWS)](https://aws.amazon.com/), and [mLab](https://mlab.com/) to produce in-depth guides and best practices for getting your Parse app running on their infrastructures.

**Amazon Web Services**

Using Amazon Web Services directly is a great choice for hosting and scaling your Parse Server. In the past few years, AWS has become the infrastructure of choice for both large enterprises and startups alike. In fact, Parse itself was built on AWS.

AWS is a secure cloud services platform providing a broad set of infrastructure products, ranging from bare metal instances to fully managed solutions. We recommend using [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) to host your Parse Server, which not only handles deployment, but also automatically manages scaling and monitoring your application. For the Parse Server database, you can go with mLab, which is a MongoDB as a service that frees you from managing the underlying infrastructure for your MongoDB instances.

To get the details on all these options, follow their [guide for deploying to AWS Elastic Beanstalk here](http://mobile.awsblog.com/post/TxCD57GZLM2JR/How-to-set-up-Parse-Server-on-AWS-using-AWS-Elastic-Beanstalk).

**Heroku**

If you are new to managing a backend stack, [Heroku](https://www.heroku.com) provides an easy-to-use platform to deploy and scale your Parse Server app. They were one of the first PaaS providers which let developers focus their time on building apps instead of maintaining infrastructures.

For the Parse Server database, we also suggest using mLab through the Heroku add-on.

To get the details, follow their [guide for deploying to Heroku and mLab here](https://devcenter.heroku.com/articles/deploying-a-parse-server-to-heroku) and read more on [their blog post](http://hrku.co/open-parse).

**And more...**

The options don’t end here. You can deploy Parse Server to any infrastructure that supports [Node.js](https://nodejs.org/en/) and [MongoDB](https://mongodb.com/), which could be as simple as a dedicated box. Check out our [Community Links](https://github.com/ParsePlatform/parse-server/wiki#community-links) to see all the tutorials and guides.