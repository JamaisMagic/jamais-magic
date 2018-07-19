---
title: "Webview problems notes"
description: "Webview problems notes"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - webview problems
---

今日还是遇到一些webview相关的问题想记录一下。

1. 还是修改docuiment.title的问题。

虽然我预计到要注意这个问题，但想不到别人的解决办法比较不那么优雅。具体如何就不说了，反正他们是由客户端提供一个方法，由js来调，以此修改app 客户端视图的title。

不是不行，但是比较蹩脚。由于本来存在的产品是这样解决的，所以他们自然就想到这样做，后来我说服他们使用客户端监听web的document title的办法来做，这样问题解决得更彻底和自然。

2. 一些奇葩Android机的奇葩问题。

一个是努比亚手机（Android 5.x）的webview居然只有在应用处启动时才能使用setTimeout，之后都不能用，要把应用kill掉重启才能用。
这样导致的问题是lodash的debounce不能用，而且好多需要setTimeout的地方都会有问题。至于为什么，实在是找不到原因。

第二个是不知道什么牌子的手机（Android 4.4.2）不能执行动态加载的js，就是使用js动态创建一个script标签，然后添加到页面的js不能执行，导致webpack打包的按需加载的js不能执行。具体什么原因还没查明，解决办法是把所有js打包到一个文件，还好那个页面比较小。但是如果要接其他网页呢？这种问题我觉得不值得解决啊。
