---
title: "Nginx配置支持CORS跨域资源共享"
description: "Nginx配置支持CORS跨域资源共享"
excerpt_separator: "<!--more-->"
last_modified_at: 2018-02-28T16:26:00+08:00
comments: true
categories:
  - cors
tags:
  - cors
  - cross origin
  - nginx
---

[参考这份配置](https://gist.github.com/JamaisMagic/357eb0b0be4d1b33ac4ab47388388483)

```nginx
map $http_origin $allow_origin {
  default "";
  "~^https?://(?:[^/]*\.)?(stevebuzonas\.(?:com|local))(?::[0-9]+)?$" "$http_origin";
}

map $request_method $cors_method {
  default "allowed";
  "OPTIONS" "preflight";
}

map $cors_method $cors_max_age {
  default "";
  "preflight" 1728000;
}

map $cors_method $cors_allow_methods {
  default "";
  "preflight" "GET, POST, OPTIONS";
}

map $cors_method $cors_allow_headers {
  default "";
  "preflight" "Authorization,Content-Type,Accept,Origin,User-Agent,DNT,Cache-Control,X-Mx-ReqToken,Keep-Alive,X-Requested-With,If-Modified-Since";
}

map $cors_method $cors_content_length {
  default $initial_content_length;
  "preflight" 0;
}

map $cors_method $cors_content_type {
  default $initial_content_type;
  "preflight" "text/plain charset=UTF-8";
}

add_header Access-Control-Allow-Origin $allow_origin;
add_header Access-Control-Allow-Credentials 'true';
add_header Access-Control-Max-Age $cors_max_age;
add_header Access-Control-Allow-Methods $cors_allow_methods;
add_header Access-Control-Allow-Headers $cors_allow_headers;

set $initial_content_length $sent_http_content_length;
add_header 'Content-Length' "";
add_header 'Content-Length' $cors_content_length;

set $initial_content_type $sent_http_content_type;
add_header Content-Type "";
add_header Content-Type $cors_content_type;

if ($request_method = 'OPTIONS') {
  return 204;
}
```

map部分写在http块里面，下面的add_header部分既可以写在http块里面，也可以写在location块里面。

关于nginx的map，可以参考[这里](http://nginx.org/en/docs/http/ngx_http_map_module.html)

至于为什么这份配置要用map，而不是简单一点地用if，我认为是因为这个原因：[If Is Evil](https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/)

> Directive if has problems when used in location context, in some cases it doesn’t do what you expect but something completely different instead. In some cases it even segfaults. It’s generally a good idea to avoid it if possible.

<site><a target="_blank" href="https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/">If Is Evil</a></site>
