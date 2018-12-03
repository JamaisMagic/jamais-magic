---
title: "Linux dig commamd"
description: "Linux dig commamd"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - linux command
  - dig
---

之前搞cdn的时候，知道了dig这个命令的使用。简单来说就是查询一个域名的解析，只要简单地dig your.domain.com即可。

默认的就是查询a记录。

后来用lets encrypt配置ssl证书的时候，需要手动设置域名的txt记录，然后用这个命令检查是否生效，只需要加-t参数。

```bash
dig -t txt your.domain.com
```

-t参数制定查询的记录类型，例如想查txt记录就给值为txt，cname记录就改为cname。


顺便说一下，如果修改的记录不生效，可能是系统dns缓存，这个时候需要刷新一下系统的dns缓存，Ubuntu 18.04为例：
```bash
sudo systemd-resolve --flush-caches
```

其他系统需要google查一下怎么弄。

最后顺便说一下，用docker跑certbot获取通配符的ssl证书，可以参考我这个命令，替换-d的参数，执行之后按提示操作即可。

当然，这个命令也是fork别人的拿来改的哈哈哈。

<script src="https://gist.github.com/JamaisMagic/cda624aa0c7ef0225fc845eb4a831334.js"></script>
