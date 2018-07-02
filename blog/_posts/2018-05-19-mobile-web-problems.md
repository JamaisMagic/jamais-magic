---
title: "记录一些移动端网页开发相关的问题（不包括webview）"
description: "记录一些移动端网页开发相关的问题（不包括webview）"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - mobile web
---

记录一些移动端网页开发相关的问题，不包括webview，但可能包括一些pc端也适用的问题。

#### 浏览器私密模式不能适用localStotage和sesstionStorage。

看场景解决，如果是需要传递信息到下一个页面，就适用url传，如果是一些缓存信息，没有缓存就请求接口，甚至可以在页面加个提示，提示用户适用正常模式。
另外判断私密模式没有正常方法，只能是在localStorage对象存在的情况下，setItem会报错，记得加try catch喔。

#### 有一些跳转中转页跳转到另外一个页面后再按返回键会又跳回去。

解决办法是中转页用location.replace()而不是用location.href = url做跳转。

#### 需要模拟hover效果。

使用touchstart，touchend，touchcancel等方法加class模拟。

#### 如何快速定位用户反馈问题。

针对接口问题，我是喜欢把提示笼统的接口返回信息后面增加接口返回的错误码，这样用户的截图或者反馈就可以知道怎么查，直接让服务端同事看。

#### 从url里面拿链接跳转需要判断url是否我们可以跳的域名再跳，不然可能有安全问题。

#### 手机打开用户发短信界面，url有兼容问题。

需要添加短信内容的，ios用&符号，Android的用?符号，如 sms:?body=xxxxxx

#### 分享链接短链接问题。

我们知道分享链接使用og标签或者某些社交app专用的meta name标签，但是假如你的链接使用了端链接服务，如goo.gl等，有的社交app不会重定向到真正的地址读取信息，如WhatsApp。注意这一点，要解决的话看连接长短重要还是读取内容重要进行取舍，毕竟人家不支持也没有办法。

#### 两个不同标签页之间需要通信。

可以使用window.onstorage事件，这个是不管标签之间是否父子关系都可以进行通信，假如标签之间有父子关系，例如用window.open打开的标签，还可以用window.opener.postMessage()方法，或者window.open()返回的子标签window对象的postMessage方法进行通信，双方监听window.onmessage事件即可。

另外需要注意，window.storage事件是针对localstorage的，sessionstorage无效，因为不同标签页的sessionstorage是不共享的。

不同标签页和不同的浏览器窗口的同一域名的cookie和localstorage都是共享的。

#### ios safari的表单元素有些默认阴影等样式。

这些样式需要去掉，可以用-webkit-appearance: none;去掉。

#### chrome浏览器记住密码的黄色背景。

需要用:-webkit-autofill增加白色阴影覆盖，需要注意的是在这个选择器覆盖背景颜色是无效的，所以要用阴影。

#### chrome浏览器最小字体问题。

今日试一试，发现中文系统最小字体已经不是12px了，而是9px，再小就不能了（除了0px）。至于英文系统，没有这个问题。不过9px也够用了。
手机，没有试。

#### iphone Safari点击返回按钮js不执行。

这个问题之前没有解决办法，应该是浏览器缓存导致的，后来查了下网上，据说可以使用onpageshow事件解决，待验证。

同时可以参考这个资料。

### input type="text" 的readonly属性。

在ios上看上去好像可以编辑的，而且点击的时候会把键盘的顶部栏弹出来，只显示一个完成按钮。
这样还以为是有什么bug呢。

### form button 的type问题。

如果button不写type，则默认为submit，所以要根据用途指定type。

### window.history.go()

正常情况，调用history.go()和history.go(0)，应该都是重新加载当前页面。
<del>但是mac上的Safari和ios上的Safari是没有反应的。</del>

纠正一下，应该是在用hash做路由的时候没反应，其他情况（包括history api）是正常的。

### ios 11的input text居然不支持点击光标到字母之间进行修改，只能点到有空格的地方修改。

> <site><a target="_blank" href="https://developer.mozilla.org/en-US/Firefox/Releases/1.5/Using_Firefox_1.5_caching">Using Firefox 1.5 caching</a></site>


### ios Safari及其webview，如果顶部栏使用position:fixed;下面的容器有margin top 的，会让body的顶部离开view port顶部，html正常。

这种情况最好把margin改为padding。
