---
title: "Debug Github Pages with Jekyll on Windows"
layout: post
date: 2017-02-13
categories: debug
---

## How to debug jekyll site blog in Windows

* Starting by search in GitHub Help found this page: [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#platform-windows)
* Then go to [Jekil installation instruction](http://jekyllrb.com/docs/windows/#installation) for Windows
* Install Jekill via chocolatey (see https://chocolatey.org/)

```
choco install ruby -y
```

* Follow [How to install github-pages](http://jekyllrb.com/docs/windows/#how-to-install-github-pages)
  * Skip *choco install ruby -version 2.2.4* step and change *- C:/tools/ruby22* in - *C:/tools/rubyXX* based on Ruby version installed
* Check [Requirements](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#requirements) and install bundler
* Checkout github-pages repository locally using git and then **go to checkout folder**
* Follow Step 2: [Install Jekyll using Bundler](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-2-install-jekyll-using-bundler)
* First update bundle [Keeping your site up to date with the GitHub Pages](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#keeping-your-site-up-to-date-with-the-github-pages-gem)
* Then to build and serve site: [Step 4: Build your local Jekyll site](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-4-build-your-local-jekyll-site)
* Browse [http://localhost:4000/blog/](http://localhost:4000/blog/)
  * Add in [Front Matter](https://jekyllrb.com/docs/frontmatter/) of index and posts md _layout: default_
* Create or update .gitignore file exclude following items:

```
.sass-cache/
Gemfile.lock
_site/
```

* Commit and push changes
* Enjoy debug locally generated jekyll blog!
