---
title: "Web performance optimization 01"
description: "Web performance optimization"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - web performance
---

之前有人反馈我们的网站加载慢，我给解释的理由域名不能备案，所以没有国内cdn节点，所以慢。

但是最近认真看了一下网站加载，觉得也不能这么慢呀（具体数字还是不方便直接写出来），所以我觉得花时间做一点点优化。


## 升级webpack

工欲善其事必先利其器，在优化之前，先把打包工具升级一下，原先那个项目用webpac2打包，现在升级为webpack4。

从webpack2/3升级为webpack4需要注意的一些东西可以参考这个文档： <a target="_blank" href="https://webpack.js.org/migrate/4/">To v4 from v3</a>

一些该删的插件可以删掉，该升级的插件也要升级。另外vue和vue-template-compiler最好也升级一下新版，不然打包的时候会报错。

## 优化打包

我们的项目，是多个html页面，每个html自身又是一个多页面应用，打包的时候是把当前html需要用的js打包成一个文件，路由页面用的js打包成单独的文件。这种策略其实也没什么问题，因为当初这个项目其实只有首页一个页面，后来页面逐渐多了，现在需要改变策略。

现在改为：

在npm包里挑选一些最为公用，最常用，最可能用到的包打包合并为一个vendor文件，共所有页面公用，这个文件期望大小不要太大也不要太小，目前gzip前是1.1m，gzip之后两百几k。同时同步代码和异步代码里公用的代码也单独打包出来。

```javascript
// optimization.splitChunks.cacheGroups

vendor: {
  name: 'vendor',
  chunks: 'all',
  priority: 0,
  test: /[\\/]node_modules[\\/]\.*(module_1|module_2)/i,
},
common_initial: {
  name: 'common_initial',
  chunks: 'initial',
  minChunks: 2,
  reuseExistingChunk: true,
  priority: 0,
},
common_async: {
  name: 'common_async',
  chunks: 'async',
  minChunks: 2,
  reuseExistingChunk: true,
  priority: 0,
},
```

另外，要留言import npm 包里的文件是否正确。刚做优化的时候，发现异步公用包达到1.8m这么大，但是我又不知道是引用的哪些包导致的，就点开文件看看，发现原来是video.js。我心想，video.js不至于这么大吧，然后在import的地方点开node_modules里的文件看看，发现import的时候import的是video.cjs.js这个文件，而这个文件是没有压缩的，所以单单这个文件就1.2m，把引用的文件改为video.min.js就可以了，这个文件四百几k，但是一个库四百几k也不小了，而且不应该成为异步公用库，因为我们的video.js只有直播和段视频播放页面用得上，所以又把这个库单独打包出来。

这样一来，总的文件大小可以减少很多，gzip之前，从12.9m减到3.8m，效果很惊人，比我原来预估的要多很多。

顺便说一下，针对修改一个文件导致其他文件hash也会被修改的问题，webpack 4 配置 optimization.runtimeChunk 为 "single"，同时文件名使用contenthash，再假一个<a target="_blank" href="https://webpack.js.org/plugins/hashed-module-ids-plugin/">HashedModuleIdsPlugin</a>

## 延迟加载

延迟加载这个听起上来很简单，但是具体如何做也是要根据页面来决定的。

例如，我们那个网站首页有三个视频，这三个视频都是接的YouTube iframe进行播放，但是他们都不在首评，而且iframe的资源太大了，不做延迟加载的话页面的onload事件会推迟好多，所以我是等页面onload后在用v-if把iframe渲染出来，这样效果很好的。

另外一些vue的async component也不是单纯用了import()就好的，最好也在组件上用v-if，等到某个适合的时间再把组件渲染出来，这样单独打包出来的js文件也是延迟加载的。


Referrers:

* <a target="_blank" href="https://webpack.js.org/migrate/4/">To v4 from v3</a>

* <a target="_blank" href="https://webpack.js.org/plugins/hashed-module-ids-plugin/">HashedModuleIdsPlugin</a>

* <a target="_blank" href="https://webpack.js.org/guides/caching/">Caching</a>

* <a target="_blank" href="https://webpack.js.org/plugins/split-chunks-plugin/#splitchunks-cachegroups-priority">SplitChunksPlugin</a>

* <a target="_blank" href="https://hackernoon.com/the-100-correct-way-to-split-your-chunks-with-webpack-f8a9df5b7758">The 100% correct way to split your chunks with Webpack</a>

* <a target="_blank" href="https://medium.com/@hpux/webpack-4-in-production-how-make-your-life-easier-4d03e2e5b081">Webpack 4 in production: How to make your life easier</a>

* <a target="_blank" href="https://itnext.io/react-router-and-webpack-v4-code-splitting-using-splitchunksplugin-f0a48f110312">Webpack (v4) Code Splitting using SplitChunksPlugin</a>

* <a target="_blank" href="https://vuejsdevelopers.com/2017/07/08/vue-js-3-ways-code-splitting-webpack/">3 Code Splitting Patterns For VueJS and Webpack</a>
