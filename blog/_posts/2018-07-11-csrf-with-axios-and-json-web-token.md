---
title: "csrf with axios and json web token"
description: "csrf with axios and json web token"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - csrf
  - axios
  - json web token
---

都说最近在处理网站的一些安全问题，自然要处理一下csrf的问题。

在网上找了一圈，决定用json web token来生成和校验token。

然后在想这个token存在哪里好，发现axios已经帮你想好了，就是它默认定了个cookie字段XSRF-TOKEN，以及http请求头字段X-XSRF-TOKEN，所以只需要网站后台往前台set-cookie，把token存在浏览器cookie里，校验的时候从http头获取即可，至于前台js如何获取cookie里的token，放在http头里，axios已经做好了，当然，这个自己手动做也不麻烦。

另外，顺便说说json web token（JWT），这个货的应用场景，大概是需要校验token的地方都可以用吧，反正csrf和用户登录token我想到的都可以用这个东西生成和校验。

具体还是参考官网吧。

* <site><a target="_blank" href="https://jwt.io">JWT</a></site>
