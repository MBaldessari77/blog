---
title: "Introdution to Container based virtualization and Docker"
layout: post
date: 2017-03-30
categories: architecture so
---

In the last period, I start to look into in [Application virtualization] also knows as __Container based virtualization__ or __Containerization__, I'm NOT an expert of virtualization, also I had some sporadical experience with virtual machines (e.g. with Azure and VMWare).

## Path

* As application developer and architect my interesting falling into [Docker], a "relatively" recent project (initial release on March 2013), which has become increasingly more popular in the developer community.
* Docker is not a virtualization layer but is a container that "automates the deployment of applications inside software containers", the Dock container contains code, runtime, system tools, system libraries – anything you can install on a server (see also [Docker Wiki]).
* This is an interesting Azure blog post wich introduce the argument [Containers: Docker, Windows and Trends], and represent Microsoft interesting and full support of this technology with [Azure Container Service].

## Pratically

* Is a great idea starting from this tutorial to become confidential with _Docker way to Container based virtualization_: [Get started with Docker].
* Dig into [DockerHub] and search from 100.000+ deployed container, open official [hello-world] container and learn more.

[Application virtualization]: https://en.wikipedia.org/wiki/Application_virtualization
[Docker]: https://github.com/docker/docker
[Docker Wiki]: https://en.wikipedia.org/wiki/Docker_(software)
[Containers: Docker, Windows and Trends]: https://azure.microsoft.com/en-us/blog/containers-docker-windows-and-trends/
[Azure Container Service]: https://azure.microsoft.com/en-us/services/container-service/
[Get started with Docker]: https://docs.docker.com/engine/getstarted/
[DockerHub]: https://hub.docker.com/
[hello-world]: https://hub.docker.com/_/hello-world/
