---
title: "Nginx security configuration"
description: "Nginx security configuration"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - nginx security
  - web security
---

今日继续搞一些安全问题，主要是在nginx做配置的方面。

能在nginx做的配置就不要在里面那层（node，python等）做了，这样分工明确，里面那层服务可以专注业务。

简单罗列一下相关配置。

```nginx
server_tokens off;

add_header X-Frame-Options SAMEORIGIN;

add_header X-Content-Type-Options nosniff;

add_header X-XSS-Protection "1; mode=block";

add_header Content-Security-Policy "default-src 'self';";
```

一些参考资料。

<site><a target="_blank" href="https://gist.github.com/plentz/6737338">Best nginx configuration for improved security(and performance)</a></site>