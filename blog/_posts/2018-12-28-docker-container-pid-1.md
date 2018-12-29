---
title: "Docker container pid 1"
description: "Docker container pid 1"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - docker
---

现在是2018年12月28日，这篇东西不是介绍docker的pid 1的问题，只是整理一下自己遇到的现状。如果想了解pid 1的问题，具体可以看下面的参考链接。

今年刚接触docker的时候，已经了解到pid 1的问题，大概是在docker里面，假如自己的程序是pid 1，接收signal之后的默认行为会被忽略，例如sigterm，sigint之类的不会终止程序。
而且，pid 1在linux系统里有特殊作用，例如接收孤儿进程，而自己跑的那些进程，一般没有为这些情况设计，所以不适合作为pid 1跑。

所以要解决这个问题，可以用<a target="_blank" href="https://github.com/krallin/tini">Tini - A tiny but valid init for containers</a>，而这个东西，在docker 1.13以上的版本已经集成进去，可以在docker run的时候加--init参数启用。在docker-compose的2.2和3.7版本也支持添加相关设置。

而我自己的程序，由于编写了sigterm的相关逻辑，所以可以正常退出程序，但是一般自己写的东西不会所有都处理这些逻辑，所以觉得tini还是用上比较好。

Referrers:

<site><a target="_blank" href="https://www.ctl.io/developers/blog/post/gracefully-stopping-docker-containers/">Gracefully Stopping Docker Containers</a></site>

<site><a target="_blank" href="https://hackernoon.com/my-process-became-pid-1-and-now-signals-behave-strangely-b05c52cc551c">My process became PID 1 and now signals behave strangely</a></site>

<site><a target="_blank" href="https://www.elastic.io/nodejs-as-pid-1-under-docker-images/">Avoid running NodeJS as PID 1 under Docker images</a></site>

<site><a target="_blank" href="https://forums.docker.com/t/what-the-latest-with-the-zombie-process-reaping-problem/50758">What the latest with the zombie process reaping problem?</a></site>

<site><a target="_blank" href="https://github.com/docker/compose/issues/5049">compose file version 3.0 removed support for init #5049</a></site>

<site><a target="_blank" href="https://github.com/krallin/tini">Tini - A tiny but valid init for containers</a></site>
