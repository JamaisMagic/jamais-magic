---
title: "Nginx body限制"
description: "Nginx body限制"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - nginx
---

之前做的一个需求需要上传图片，功能弄完了随便拿几张图片测试觉得没问题就算了，后来找到一张骚微大点的文件试试发现服务器返回413，body entity too large。
一看就知道是nginx做了限制，然后搜了搜nginx的官方文档，就是在这个client_max_body_size设置，不设置的话默认是1m，这也太小了，然后我改成100m，就可以正常工作了。
不过只是我这边的服务器正常工作而已，后端的服务器也有一样的问题，想不到他们也没注意到这个问题。

> <site><a target="_blank" href="http://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size">client_max_body_size</a></site>
