---
title: "Debug Github Pages with Jekill on Windows (work in progress)"
layout: default
date: 2017-02-13
categories: debug
---

* Starting by search in GitHub Help found this page: [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#platform-windows)
* Then go to [Jekil installation instruction](http://jekyllrb.com/docs/windows/#installation) for Windows
* Install Jekill via chocolatey (see https://chocolatey.org/)

```
choco install ruby -y
```

* Follow [How to install github-pages](http://jekyllrb.com/docs/windows/#how-to-install-github-pages)
  * Skip *choco install ruby -version 2.2.4* step and change *- C:/tools/ruby22* in - *C:/tools/rubyXX* based on Ruby version installed

* First update bundle [Keeping your site up to date with the GitHub Pages](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#keeping-your-site-up-to-date-with-the-github-pages-gem)

* Then to build and serve site: [Step 4: Build your local Jekyll site](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-4-build-your-local-jekyll-site)
