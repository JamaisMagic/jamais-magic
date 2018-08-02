---
title: "Node express static"
description: "Node express static"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - express static
---

今日帮一个网站申请cdn。本来那个网站不是我管理的。不过本着助人为乐的精神，我就接手这个事情，而且这些对我来说算是轻车熟路了。反正大概就是需要转发Host的http头，然后接口相关的目录所有头都转发，然后想利用Accept配置支持webp就转发Accept。

然后按照我的习惯，其他都是根据源站策略缓存，然后自己在nginx配置相关缓存策略。

但是那个网站貌似没有用nginx。

只是用了node。

然后我就看看express static相关的设置，可以参考express的官网。

大概配置了html文件的缓存时间短写，其他的长些。

然后简单看了看static.mime.lookup()这个方法，发现是根据文件名判断mime type的，应该不存在性能问题，又可以满足需求，可以放心用。
