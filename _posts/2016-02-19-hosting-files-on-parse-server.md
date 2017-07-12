---
id: 3935
title: Hosting Files on Parse Server
date: 2016-02-19T13:31:52+00:00
author: patrickpelletier
layout: post
guid: http://blog.parse.com/?p=3935
permalink: /announcements/hosting-files-on-parse-server/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
categories:
  - Announcements
---
[Parse Server](https://github.com/ParsePlatform/parse-server), the open source Parse backend, initially shipped with a GridStore adapter as the only option to store your files. This uses your MongoDB database to save file content and is a great way to get you started quickly with minimal effort and dependencies. However, for heavy production usage, it's advisable to use a more scalable data store, optimized for storing files and that won't put additional load on your MongoDB instance.

### Hosting on AWS S3

Recently, we added support for AWS S3 as the backing store for your files. S3 is what we used internally in Hosted Parse. It's widely used and can scale for pretty much all Parse Server applications.

Since setting up and configuring Parse Server to use the S3 files adapter is a little more involved than using the default adapter, a lot of people asked us for a guide on the topic. The detailed instructions are in the [wiki](https://github.com/ParsePlatform/parse-server/wiki/Storing-Files-in-AWS-S3):

<ul class="standard-list">
  <li>
    <a href="https://github.com/ParsePlatform/parse-server/wiki/Storing-Files-in-AWS-S3#setup-your-bucket-and-permissions">Setup your S3 bucket and permissions</a>
  </li>
  <li>
    <a href="https://github.com/ParsePlatform/parse-server/wiki/Storing-Files-in-AWS-S3#using-the-s3adapter">Use the S3 file adapter in your Node/Express application</a>:
  </li>
</ul>

<pre class="line-numbers"><code class="language-javascript">
var S3Adapter = require('parse-server').S3Adapter;
var api = new ParseServer({
...
filesAdapter: new S3Adapter(
"AWS_ACCESS_KEY_ID",
"AWS_SECRET_ACCESS_KEY",
"BUCKET_NAME",
{ directAccess: true }
),
...
});</code></pre>

### Existing files on Hosted Parse

When using Parse Server, any new file you create will be saved on the data store you select through the files adapter (MongoDB, S3, ...). However, when you migrate your application to your own MongoDB instance, your existing files are still in Parse's hosted service. As long as you specify the correct **fileKey**, Parse Server knows exactly how to access them, so they will keep working just fine.

Another question we get all the time is: What happens to those files after Hosted Parse is retired on January 28, 2017? Unfortunately, our S3 bucket will also be turned down, which means those files will need to be migrated before that date. You may use the [files utility](https://github.com/parse-server-modules/parse-files-utils) to perform this operation.

### Get Involved

The community has already been an huge driver of innovations for Parse Server. As an example, we never had support for deleting files on Parse's hosted service. Thanks to [contributions](https://github.com/ParsePlatform/parse-server/pull/354) from [westhom](https://github.com/westhom), this is now possible exclusively on Parse Server since version 2.1.0.

We look forward to how we can improve file handling in Parse Server together. We can probably expect many more files adapters in the next few months for other cloud object stores. New adapters are pretty easy to implement; there's only 4 functions to implement for [the interface](https://github.com/ParsePlatform/parse-server/blob/master/src/Adapters/Files/FilesAdapter.js). A file adapter to save to the local filesystem would be an awesome first pull request!