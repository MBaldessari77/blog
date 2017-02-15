---
title: "Debug Github Pages with Jekill on Windows (work in progress)"
layout: default
date: 2017-02-13
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

*   Search and follow this instruction [ERROR: Could not find a valid gem 'jekyll' (>= 0), here is why:](https://github.com/juthilo/run-jekyll-on-windows/issues/34)

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
*   Continue with step Requirements(https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#requirements)
*   Checkout the master branch of site (if github pages is mapped on master) and go in the checkouted folder
*   Continue to step [Install Jekyll using Bundler](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-2-install-jekyll-using-bundler)

```
bundle install
Fetching gem metadata from https://rubygems.org/..............
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Resolving dependencies...
Using public_suffix 2.0.5
Using colorator 1.1.0
Using ffi 1.9.17 (x64-mingw32)
Using forwardable-extended 2.6.0
Using sass 3.4.23
Using rb-fsevent 0.9.8
Using liquid 3.0.6
Using mercenary 0.3.6
Using rouge 1.11.1
Using safe_yaml 1.0.4
Using bundler 1.14.4
Gem::RemoteFetcher::FetchError: SSL_connect returned=1 errno=0 state=SSLv3 read
server certificate B: certificate verify failed
(https://rubygems.org/gems/i18n-0.8.0.gem)
An error occurred while installing i18n (0.8.0), and Bundler cannot continue.
Make sure that `gem install i18n -v '0.8.0'` succeeds before bundling.
PS D:\Sviluppo\blog>
```

To skyp last error modify temporarly Gemfile and change protocol source to http

```
source 'http://rubygems.org'
```

Then rerun "bundle install"

```
bundle install
Fetching gem metadata from http://rubygems.org/............
Fetching version metadata from http://rubygems.org/...
Fetching dependency metadata from http://rubygems.org/..
Resolving dependencies...
Installing i18n 0.8.0
Installing json 1.8.6 with native extensions
Installing minitest 5.10.1
Installing thread_safe 0.3.5
Using public_suffix 2.0.5
Installing coffee-script-source 1.12.2
Installing execjs 2.7.0
Using colorator 1.1.0
Using ffi 1.9.17 (x64-mingw32)
Installing multipart-post 2.0.0
Using forwardable-extended 2.6.0
Installing gemoji 3.0.0
Installing net-dns 0.8.0
Using sass 3.4.23
Using rb-fsevent 0.9.8
Installing kramdown 1.11.1
Using liquid 3.0.6
Using mercenary 0.3.6
Using rouge 1.11.1
Using safe_yaml 1.0.4
Installing mini_portile2 2.1.0
Installing jekyll-paginate 1.1.0
Installing jekyll-swiss 0.4.0
Installing minima 2.0.0
Installing unicode-display_width 1.1.3
Using bundler 1.14.4
Gem::InstallError: The 'json' native gem requires installed build tools.

Please update your PATH to include build tools or download the DevKit
from 'http://rubyinstaller.org/downloads' and follow the instructions
at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'

An error occurred while installing json (1.8.6), and Bundler cannot continue.
Make sure that `gem install json -v '1.8.6'` succeeds before bundling.
```

We need to install last ruby [Development-Kit installer](http://rubyinstaller.org/downloads/archives) for Windows

*   To be continued and reviewed
