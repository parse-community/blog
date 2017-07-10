# Parse Community Blog [WIP]

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
  davimacedo:
    name: Davi Macedo
    byline: Co-founder at Back4App
    github: davimacedo
    twitter: adavimacedo
    site: https://www.back4app.com
```

Everything but name is optional.

## Authoring an Article

To generate a new post, create a new file in the `_posts` directory. Be sure to add your name as the author of the post and include several categories to file the post under. Here is a sample header:

```yaml
layout: post
title: Welcome to the new Parse Blog
date: 2017-07-06 13:08 -0700
comments: true
author: davimacedo
categories: [Announcements, Learn, Events, Customers, Videos]
```

More info can be found in the [official docs](http://jekyllrb.com/docs/posts/).
