---
title: "Debug webview的一些经验"
description: "Debug webview的一些经验"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - webview debug
---

webview的调试，大致上跟调试手机端网页差不多。

不过也有些不同。

Android 4.4以上的手机，默认不能用Chrome远程调试，需要在客户端添加允许调试。

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    WebView.setWebContentsDebuggingEnabled(true);
}
```

iOS可以在mac上用Safari调试，如果普通版本的Safari不行，就用Technology Preview版本。

另外，iOS9还是10应默认block掉不安全的网络请求，例如http网页不能加载，需要修改Info.plist配置。

顺便吐槽一下好多人不知道Android studio可以在logcat查看webview用console打出来的log，通过过滤chromium或者console就可以看到。
但是xcode没有这种功能。

最后，自己调试不麻烦，麻烦的是跟异地的人调试，例如之前跟台北同事调试，还好在我的带领下，一切都很顺利。

异地调试，首先要自己对客户端webview熟悉，有些问题，直觉就能知道原因，例如客户端没有添加允许执行js，没有允许是用domStorage等。
其他一些需要教会他们怎么看log，让他们帮忙看log，如果嫌这个麻烦，还可以自己做一个简单的后台，页面把log信息通过网络发送到那个后台，省去教他们的麻烦。
最紧要的一点是，要对客户端与web交互的方案熟悉。通常新项目的客户端与web交互的方案都是我出的，js的sdk也是我写的。
最后，如果如果教他们不会，也没空写后台，可以考虑自己写个webview实现一遍定义的接口，至少可以证明web端没有问题。然后再看看其他地方是否有什么问题。
