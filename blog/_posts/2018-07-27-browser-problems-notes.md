---
title: "Browser problems notes"
description: "Browser problems notes"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - Browser problems
---

今日还是遇到一些webview相关的问题想记录一下。

1. Safari 音频不能自动播放问题，正常处理click可以，但是如果click事件里有异步操作之后再播放，浏览器报错。解决办法是在监听用户激活事件（click，touchstart等）上直接执行音频的play方法，可以是一个空src的，或者设置volume为0，后面再正常播放。

2. Safari的input光标从底部对准base line，但是顶部对准输入框顶部，而不是文字顶部。解决办法是设置line-height:normal;或者1、100% 。

3. WeChat 的webview不支持audio的src设置urlObject，播放没声音，根据网上的资料，可能不是很新的Android版本都不行呀，亲测Android4.4确实不行，一些新系统加入app 用的webview版本较旧应该也不行。解决办法可以是进行版本检测chrome >= 52。WeChat的chrome（57）比qq的chrome（53）版本新也不行，估计还要检测平台，例如微信。

> <site><a target="_blank" href="https://bugs.chromium.org/p/chromium/issues/detail?id=253465">Videos on Android do not play when the src is set as a blob via create URL</a></site>
