---
title: "构建es module相关的代码"
description: "构建es module相关的代码"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - es module
---

最近看了下vue-cli 3.0的文档，发现可以构建不转换为es5的代码，即是可以构建两份代码，一份转换为es5，一份直接用es6+。
觉得es6自2015年发布以来，已经过去三年了，普及程度应该不低，浏览器的支持度应该也不低，等vue-cli 3.0正式版release的时候可以试试这么用哈。

简单了解一下，大概是利用支持type="module"属性的浏览器几乎都支持大部分es新特性这个情况来实现的。
添加两个script标签，一个有type="module"属性，一个有nomodule属性，支持type="module"的会忽略nomodule，而旧浏览器则加载nomodule里的脚本。

具体可以参考下面的文章。

<site><a target="_blank" href="https://philipwalton.com/articles/deploying-es2015-code-in-production-today/">Deploying ES2015+ Code in Production Today</a></site>

另外，关于es module在浏览器端的一些特性，可以看这篇文章了解一下。

<site><a target="_blank" href="https://jakearchibald.com/2017/es-modules-in-browsers/">ECMAScript modules in browsers</a></site>

<site><a target="_blank" href="https://developers.google.com/web/fundamentals/primers/modules">Using JavaScript modules on the web</a></site>


顺便，还可以了解一下node使用的commonjs module的一些细节。

<site><a target="_blank" href="https://medium.freecodecamp.org/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8">Requiring modules in Node.js: Everything you need to know</a></site>

至于es module在node端的一些细节，下次再补补。
