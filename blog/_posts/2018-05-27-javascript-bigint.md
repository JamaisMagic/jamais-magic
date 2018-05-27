---
title: "javascript BigInt"
description: "javascript BigInt"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - javascript BigInt
---

之前做项目开发遇到的一个问题，估计很多人都遇到过。就是服务端返回一个用户id，是一个数字id，但这个数字很大，后面看到的都是0，其实是这个数字超出了js能处理的范围，导致精度损失，最后解决方法是返回字符串。

最近js准备支持这种超大数字，具体可以参考下面链接。

> <site><a target="_blank" href="https://github.com/tc39/proposal-bigint">tc39/proposal-bigint</a></site>

> <site><a target="_blank" href="https://developers.google.com/web/updates/2018/05/bigint">BigInt: arbitrary-precision integers in JavaScript</a></site>
