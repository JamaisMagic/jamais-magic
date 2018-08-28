---
title: "Group and count grep result"
description: "Group and count grep result"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - grep
  - sort
  - uniq
---

今日一次过认识了几个linux命令。

大概是需要做用一件这样的事：筛选一个日志文件里面的ip地址，并统计不同的ip地址出现的次数。

想用Linux命令实现，而不是写脚本。

所以网上搜了一下，大概结论是可以这样实现：

```bash
grep -o '\s[0-9]*\.[0-9]*\.[0-9]*\.[0-9]*\s' file.log | sort | uniq -c
```

ip地址的正则表达式可以忽略，我只是写个简单够用的。

grep就不多说了，平时用得多， -o参数表示只输出匹配的内容（而不是默认的输出整行）。

sort命令就是把内容排序，这会使得重复出现的内容都排到相邻的行。

uniq命令就是去掉相邻的重复内容，-c参数是统计相邻重复内容的数目，并添加到改内容的前面。

大概就是这样实现，今日一次认识两个命令sort和uniq，得闲有需要的时候可以Google一下相应的文档看看更详细的用法。
