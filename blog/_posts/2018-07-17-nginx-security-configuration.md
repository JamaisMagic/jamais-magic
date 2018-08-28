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

add_header Content-Security-Policy "default-src 'self';" always;

add_header Strict-Transport-Security "max-age=86400; includeSubdomains; preload";
```

其中Strict-Transport-Security这个头应该只需要在https访问的情况下加，http加了浏览器也是忽略，因为浏览器不信任http。

一些参考资料。

<site><a target="_blank" href="https://gist.github.com/plentz/6737338">Best nginx configuration for improved security(and performance)</a></site>

content-security-policy

<site><a target="_blank" href="https://content-security-policy.com/">Content Security Policy (CSP)
Quick Reference Guide</a></site>
