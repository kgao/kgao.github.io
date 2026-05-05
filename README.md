Kev's Tech Blog
=========================
[![Coverage Status](https://s3.amazonaws.com/assets.coveralls.io/badges/coveralls_100.svg)](#)

## Quick Start (Local)

This site is built with [Jekyll](https://jekyllrb.com/) and served by GitHub Pages. To preview changes locally before pushing:

### Prerequisites
- **Ruby** 3.x (on Windows, install [RubyInstaller](https://rubyinstaller.org/) — pick a `Ruby+Devkit` build)
- **Bundler**: `gem install bundler`

Verify:
```pwsh
ruby --version
bundler --version
```

### One-time setup
If there's no `Gemfile` in the repo root, create one with:

```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

Then install dependencies:
```pwsh
bundle install
```

### Run the site
```pwsh
bundle exec jekyll serve --livereload
```

Open http://localhost:4000 in your browser.

Useful flags:
- `--drafts` — preview posts under `_drafts/`
- `--future` — render posts dated in the future (needed if your post date is ahead of your local clock)

### Docker alternative (no Ruby install needed)
```pwsh
docker run --rm -it -p 4000:4000 -v ${PWD}:/srv/jekyll jekyll/jekyll jekyll serve
```

### Add a new blog post
Create a file under `_posts/blogs/` named `YYYY-MM-DD-Title.markdown` using the frontmatter shown below. Place any images under `img/blog/` and reference them with a relative path.

For more, see the [GitHub Pages + Jekyll guide](https://help.github.com/articles/using-jekyll-with-pages/).

## How to update
 - Place a image in `/img/portfolio/`
 - Create posts to display your projects. Use the follow as an example:
```txt

layout: blog
category: blogs
title: 'Welcome to My Dev Blog'
date: '2014-11-19'
author: Kgao
tags: 'Github,Marketdown'

```

 - Powered-by
  [Jekyll](http://jekyllrb.com/) |
  [bootstrap](http://getbootstrap.com/) |
  [NYMag Blog](http://nymag.github.io)
