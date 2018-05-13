---
title: "关于google第三方验证登录"
description: "关于google第三方验证登录"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - google auth
---

Google早就不允许用webview进行第三方登录。当网站在webview里打开，点击google登录跳转到google的时候，会返回403 forbidden。

> In the coming months, we will no longer allow OAuth requests to Google in embedded browsers known as “web-views”

> <site><a target="_blank" href="https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html">Modernizing OAuth interactions in Native Apps for Better Usability and Security</a></site>

Google为什么这么干，原因大概就是为了提高用户体验。

解决办法最好是检测到网站在webview里打开时，提示用户需要在浏览器打开网站才能使用google登录。

那么，到底如何检测用户是用webview打开网站的呢？

具体的还是用user agent字符串吧，可以参考google官方文档。

> <site><a target="_blank" href="https://developer.chrome.com/multidevice/user-agent">User Agent Strings</a></site>

Android4.4以上的webview是可以区分的，至于4.4以下的没看到什么明显能区分的地方，估计不好分，不过用4.4以下机器的越来越少了，其实可以忽略。

至于ios的参考网上找的这段代码。

> <site><a target="_blank" href="https://codepen.io/RhinoLu/pen/RrPxMv">iOS webview detect</a></site>

```javascript
var is_uiwebview = /(iPhone|iPod|iPad).*AppleWebKit(?!.*Safari)/i.test(navigator.userAgent);

document.body.innerHTML += "<p>ios webview : " + is_uiwebview + "</p>";
document.body.innerHTML += "<p>" + navigator.userAgent + "</p>";
```
