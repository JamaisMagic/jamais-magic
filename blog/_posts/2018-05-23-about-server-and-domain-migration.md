---
title: "关于前端网站的服务器迁移以及域名修改"
description: "关于前端网站的服务器迁移以及域名修改"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - server migration
  - domain
---

这两日部门领导要求一个刚做没多久的网站更换域名，而且着急上线。那就开始干。

首先内部服务器不用动，如ec2 elb这种。

重新申请个cdn，回源到elb。

重新申请https证书，部署到cdn。

新域名解析到cdn。

然后可以测试新域名。

原来的网站检查一下是否有些链接跳转或者接口调用的写死域名的，尤其多人合作的网站没有定好规范各种写法千奇百怪，有的话先改过来。

同时也要检查网站logo，图片资源之类的是否带有网站域名，也要换过来。

测试没问题后在服务器如nginx配置个301（或者308？）重定向跳转到新域名。

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name old.domain.com;
    return 301 https://new.domain.com$request_uri;
}
```

另外让我想起之前做过网站服务器迁移，一起在这里做个记录。

迁移网站跟更换域名有点不同，迁移网站主要修改内部的东西。

首先申请新的机器，建个新的代码部署（Jenkins），把代码部署到新机器（旧的代码部署还需要部署到旧机器）。

再申请个lb，指向刚才的机器。

再申请个cdn，回源到lb。

测试没问题后，修改域名解析。
