---
title: "使用X-Forwarded-Proto获取客户端访问网站使用的协议"
description: "使用X-Forwarded-Proto获取客户端访问网站使用的协议"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - X-Forwarded-Proto
  - protocol
  - schema
  - nginx
  - $http_x_forwarded_proto
---

之前做自己的网站，需要知道用户使用什么协议访问网站，如果是http就重定向到https，实现很简单，直接监听80端口做跳转即可。
后来做公司的网站，想实现类似功能，不过情况有点不一样，因为公司网站有cdn，有lb，nginx不是直接暴露在外面，所以不能像上面说的做。

所以我在nginx的后端一层（node）把获取的http头打印出来，想看看效果（X-Forwarded-Proto），但是发现获取到的是http，当时我的nginx的配置大概是，
```nginx
proxy_set_header X-Forwarded-Proto: $schema;
```

然后查了一下网上，发现这个应该还是使用在nginx直接暴露的网站里的，应该用 $http_x_forwarded_proto 这个变量就差不多了，不过我还未试呢。

> <site><a target="_blank" href="https://oanhnn.github.io/2016-02-29/how-to-force-https-behind-aws-elb.html">How to force HTTPS behind AWS ELB</a></site>

另外有一点需要注意的是，lb通常会有作为健康检查的请求，不要把那个请求重定向到https喔。
