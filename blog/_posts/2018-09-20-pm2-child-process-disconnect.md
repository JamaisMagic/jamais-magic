---
title: "pm2 child process disconnect"
description: "pm2 child process disconnect"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - pm2
  - disconnect
---

之前项目改个东西，上了测试环境，发现有个新接口访问时不时404，很奇怪。

登录到测试机器看一看，发现居然有10个node进程，正常是4个，而且只有4个是发布之后才起来的进程，其他的都是老进程。

所以是pm2（v2.9.3）把一些请求发到老的进程，然后老进程没有新接口就404了？

于是我看了看pm2的log，发现有个比较关键的是：

pm2 id:_old_2 disconnected，

pm2 Unknown id _old_1，

但是google了下似乎没有找到很确切的原因，也许原因多种多样。

然后我把所有node进程kill掉重启。

感觉pm2还是不能让人省心（之前遇到过pm2自己crash掉的情况），看看后面会不会再遇到其他问题，视情况要用supervisord替换pm2.
