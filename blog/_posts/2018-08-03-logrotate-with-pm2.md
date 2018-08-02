---
title: "Logrotate with pm2"
description: "Logrotate with pm2"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - logrotate pm2
---

前两日要把node的日志导给服务端参考，主要是ip和ua，然后自己下载下来，整理了一下再给到对方。

整理主要是grep出相应日期的行，写到新文件再发给对方。

为什么要这么做？因为日志没有做切割。

为什么没有做切割？因为当初没什么时间搞，网站量也小，就先搁置。

现在看来还是需要搞好这件事，但是都是用的写业务的额外时间搞。= =

既然要搞日志切割，自然想到用Logrotate啦。

