---
title: "Debug Github Pages with Jekill on Windows (work in progress)"
layout: default
date: 2017-02-11
categories: debug
---

*   Starting by search in GitHub Help found this page: [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#platform-windows)
*   Then go to [Jekil installation instruction](http://jekyllrb.com/docs/windows/#installation) for Windows
*   Install Jekill via chocolatey (see https://chocolatey.org/)

```
choco install ruby -y
```

*   Reopen cmd

```
gem install jekyll
ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
          Unable to download data from https://rubygems.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://api.rubygems.org/specs.4.8.gz)
```

*   Follow this instruction [ERROR: Could not find a valid gem 'jekyll' (>= 0), here is why:](https://github.com/juthilo/run-jekyll-on-windows/issues/34)

```
gem sources --remove https://rubygems.org/
    https://rubygems.org/ removed from sources
gem sources -a http://rubygems.org/
    https://rubygems.org is recommended for security over http://rubygems.org/
Do you want to add this insecure source? [yn]  y
    http://rubygems.org/ added to sources
gem install jekyll
    ...
```

*   Install Jekyll using Bundler

Checkout and go in master branch of site (if github pages is mapped on master)

*   To be continued and reviewed
