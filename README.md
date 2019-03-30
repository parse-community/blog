# Parse Community Blog

[![Join The Conversation](https://img.shields.io/discourse/https/community.parseplatform.org/topics.svg)](https://community.parseplatform.org/c/parse-server)
[![Backers on Open Collective](https://opencollective.com/parse-server/backers/badge.svg)](#backers)
[![Sponsors on Open Collective](https://opencollective.com/parse-server/sponsors/badge.svg)](#sponsors)
[![License][license-svg]][license-link]
![Twitter Follow](https://img.shields.io/twitter/follow/ParsePlatform.svg?label=Follow%20us%20on%20Twitter&style=social)

## Setup

To run the site locally, you'll need Jekyll and the GitHub Pages gem. The GH Pages gem is required to provide your local site with a similar environment to prod. For example, the `site.github` param is [automatically provided by GitHub](https://help.github.com/articles/repository-metadata-on-github-pages/) in prod.

### Prerequesites

* Ruby 2.1 or higher;
* Jekyll 3 or higher;
* Bundler (`gem install bundler`).

### Running

```
bundle install
bundle exec jekyll serve
```

Open http://127.0.0.1:4000/ in your web browser.

## Contributing

Send your contribution via PR. Once it is merged, it will be automatically published to the site.

## Adding an Author

Authors are key-value stored, so you will need to give yourself a key inside [_config.yml](_config.yml) - for example:

```yaml
  flovilmart:
    name: Florent Vilmart
    byline: Core Contributor on Parse
    github: flovilmart
    twitter: flovilmart
    site: http://parseplatform.org/
```

Everything but name is optional.

## Authoring an Article

To generate a new post, create a new file in the `_posts` directory. Be sure to add your name as the author of the post and include several [categories](#using-categories--adding-new-ones) if appropriate. Here is a sample header:

```yaml
layout: post
title: Welcome to the new Parse Blog
date: 2017-07-06 13:08 -0700
comments: true
author: flovilmart
categories: [Announcements, Learn, Events, Customers, Videos]
```

More info can be found in the [official docs](http://jekyllrb.com/docs/posts/).

## Using categories & adding new ones

When adding a category to a blog post please remember to capitalize words. For categories with multiple words please separate words with dashes instead of spaces eg. New-Year not New Year.

The current list of categories:
- Announcements
- Community
- Customers
- Events
- GitHub
- JavaScript
- Learn
- New-Year
- NodeJS
- Notice
- PHP
- Release
- SDK
- Security
- Update
- Videos
- Tutorial
- Open-Source
- Hacktoberfest
- Engineering
- Design

If you would like to use a new category please make a new file in the [categories folder](blog/categories), for example:
```yaml
---
layout: blog
permalink: /blog/categories/announcements/
pagination:
    enabled: true
    category: Announcements
    permalink: /:num/
---
```

[license-svg]: https://img.shields.io/badge/license-BSD-lightgrey.svg
[license-link]: https://github.com/parse-community/Parse-SDK-JS/blob/master/LICENSE
