---
title: "CII unit test error debug using Docker & Visual Studio code (eg. System.Path)"
layout: post
date: 2018-06-17
categories: tip cross-platform code-snippet net.core c# docker
---

# Introduction

Actually, I use [Travis CI](https://travis-ci.org/MBaldessari77) for Continuous Integration & Testing of my small .NET open projects.
Keep in mind that Travis CI by default uses an image of Ubuntu Linux distribution to implement Continuous Integration on your software.

## Problem

Some time ago I had a problem with a new service, for suggesting File System path, created on my [FxCommonStandard Library](https://github.com/MBaldessari77/FxCommonStandard).
I had some test wrong on Travis CI but not in my VS.2017 using ReSharper unit test runner... but why?
So I choose to use an Ubuntu Docker image for install Visual Studio Code and debug test, it was easier to do than to explain.

So I discovered that I had to use a constant in the tests and that the error was not in the service (then ulteriorly refactored).

```csharp
Path.DirectorySeparatorChar
```

## Conclusion

Creating a multiplatform software is a great idea, but do not think that the simple transition to .NET Core will not be painless.
Correct unit test e/o integration e/o smoke e/o human UAT it will be better to plan it as contingency before thinking to a migration.
Also, Docker seems to be a very powerful & lightweight alternative to system virtualization.
