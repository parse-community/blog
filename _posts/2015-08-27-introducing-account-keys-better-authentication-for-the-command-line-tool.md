---
id: 3755
title: 'Introducing Account Keys: Better Authentication for the Command Line Tool'
date: 2015-08-27T10:54:25+00:00
author: pavanathivarapu
layout: post
guid: http://blog.parse.com/?p=3755
permalink: /announcements/introducing-account-keys-better-authentication-for-the-command-line-tool/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "4072662491"
categories:
  - Announcements
tags:
  - accountkeys
  - commandlinetool
  - security
---
At Parse we're always striving to make it easier for developers to build great apps. Part of this mission is to provide secure and easy to use tools.

Today, we're excited to announce **Account Keys**,** ******which are personal access tokens for API and command line usage. Account Keys are similar to [GitHub personal API](https://github.com/blog/1509-personal-api-tokens) tokens. As part of this update we've also strengthened the security of the Parse command line tool.

* * *

## 

## Security

<p class="unchanged rich-diff-level-one">
  Along with releasing Account Keys, we've changed how the Parse command line tool sets up your app's directory, <code>parse new</code> will no longer create a config file that stores your app's <code>master key</code>. Instead, the Parse command line tool will fetch the <code>master key</code> whenever required using the <a href="https://parse.com/docs/rest/guide#apps">apps API</a>, and by authenticating via a configured account key. This is a more secure process and helps prevent accidentally exposing your app's <code>master key</code>.
</p>

For existing apps, please check out the [docs](https://parse.com/docs/ios/guide#command-line-secure-config-format) for how to migrate to this new config format.

## Ease of Use

Account Keys eliminate the need for you to enter your Parse account's email and password each time you run a command that requires your app's `master key`, for instance when deploying an app with the `parse deploy` command. Instead you only need to configure an account key once, and the Parse command line tool will store it by default and retrieve it automatically as needed.

Another improvement is that if you're a Parse developer who has created a Parse account via third party login such as Facebook, you are no longer forced to set a Parse account password. The command line tool will automatically use a configured account key when authentication is required.

See below for how you can easily configure a default account key.



* * *

&nbsp;

To start using these new features, upgrade your command line tool to the latest version by running `parse update`.

Please check out our [docs](https://parse.com/docs/js/guide#command-line-account-keys) to learn more. We'd love to hear your feedback -- comment below or visit [GitHub issues](https://github.com/ParsePlatform/parse-cli/issues).