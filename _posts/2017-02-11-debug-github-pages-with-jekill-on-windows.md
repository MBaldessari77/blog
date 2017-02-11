---
title: "Debug Github Pages with Jekill on Windows"
layout: default
date: 2017-02-11
categories: debug
---

1. Starting by search in GitHub Help found this page: [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#platform-windows)

2. Then go to [Jekil installation instruction](http://jekyllrb.com/docs/windows/#installation) for Windows

2. 1 Install Jekill via chocolatey (see https://chocolatey.org/)

```
npm install -g jshint
```

2. 2 Reopen cmd

```
gem install jekyll
ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
          Unable to download data from https://rubygems.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://api.rubygems.org/specs.4.8.gz)
```

2. 3 Follow this instruction [ERROR: Could not find a valid gem 'jekyll' (>= 0), here is why:](https://github.com/juthilo/run-jekyll-on-windows/issues/34)

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

4. Follow [Step 1: Create a local repository for your Jekyll site](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-1-create-a-local-repository-for-your-jekyll-site) OK
5. 
