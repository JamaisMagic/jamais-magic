---
title: "nginx default server"
description: "nginx default server"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - nginx
---

之前配置nginx，想想设置个默认server，所有非指定的server都走那个默认server，隐隐约约记得默认server是下划线 _ 。

但是我特意Google搜了一下，发现不是下划线，下划线的意思应该是匹配不到的server，标准做法应该是listen个default_server，具体看看这篇文章呗。

顺便提一下，nginx那些ssl、http2都是在listen指令里设置，尤其是ssl on这个指令已经deprecated。

> <site><a target="_blank" href="https://blog.gahooa.com/2013/08/21/nginx-how-to-specify-a-default-server/">nginx: how to specify a default server</a></site>

> <site><a target="_blank" href="http://nginx.org/en/docs/http/ngx_http_core_module.html#listen">Module ngx_http_core_module</a></site>
