---
title: "target _blank 的安全问题"
description: "target _blank 的安全问题"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - target="_blank"
  - rel="noopener"
---

之前做项目，很多地方用到target="_blank"的链接，从来没有考虑过会有什么安全问题，知道最近用react的时候，构建工具提示target="_blank"会有安全问题，建议增加属性rel="noopener noreferrer"，才知道会有安全问题。

这个问题大概就是在新打开的标签页，可以通过window.opener.location访问访问opener，它可以对location进行赋值（无论跨域还是同域）。例如改写window.opener.location.hash，如果网站有些内容会把hash渲染到页面，而没有做xss防御，就可能被别人利用了。应该算是xss攻击的一种手段吧。

具体可以参考下面的链接。

* <site><a target="_blank" href="https://mathiasbynens.github.io/rel-noopener/">About rel=noopener</a></site>

另外，文中推荐了另外一篇文章，讨论什么时候用target="_blank"的，也可以顺便看看。

* <site><a target="_blank" href="https://css-tricks.com/use-target_blank/">When to use target=”_blank”</a></site>
