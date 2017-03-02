---
title: "Syncing forked repository with git"
layout: post
date: 2017-03-02
categories: git tips
---

## Pratically

1. Configuring a remote for a fork (example applied to my actual source jekyll theme)
```powershell
git clone https://github.com/waldrix/cayman.git
cd .\cayman\
git remote add upstream https://github.com/pages-themes/cayman
```
2. Merge the newly created upstream remote
```powershell
git fetch upstream
git checkout master
git pull
git merge upstream/master
```
3. Merge & Commit with git