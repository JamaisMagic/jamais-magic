---
title: "记录一些Android webview相关的问题"
description: "记录一些Android webview相关的问题"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - Android webview
---

##### 页面js没有运行。

其实就是webview没有启用js，需要启用。

```java
WebSettings settings = mWebView.getSettings();
settings.setJavaScriptEnabled(true);
```

##### 不能用localStarage。

需要webview开启web存储。

```java
settings.setDomStorageEnabled(true);
```

另外，如果需要使用数据库，应该也是要webview开启的，不过我自己没有用过，没有验证。

```java
settings.setDatabaseEnabled(true);
if(Build.VERSION.SDK_INT < Build.VERSION_CODES.KITKAT) {
  mWebView.getSettings().setDatabasePath("/data/data/" + mWebView.getContext().getPackageName() + "/databases/");
}
```

##### 动态修改网页title。

例如做一些单页应用的时候，用webview加载网页，顶部栏属于客户端部分，会显示网页title。用户点击按钮跳到别的界面，需要修改网页title，但是客户端可能只是读取第一次进入页面时的title，例如在onPageFinished方法里面读view.getTitle()，之后即使用js修改了document.title也不管用，这种时候需要客户端监听title的变化，改变顶部栏的文案。其实可以两种一起用啦。

```java
webview.setWebChromeClient(new WebChromeClient() {
    @Override
    public void onReceivedTitle(WebView view, String title) {
        super.onReceivedTitle(view, title);
        if (!TextUtils.isEmpty(title)) {
            YourActivity.this.setTitle(title);
        }
    }

    @Override
    public void onReceivedIcon(WebView view, Bitmap icon) {
        super.onReceivedIcon(view, icon);
        setIcon(icon);
    }
});
```

##### 点击后退按钮直接关闭Activity，而不是按网页历史记录后退。

这个也需要客户端修改，在Activity的onBackPressed回调重判断webview能否goBack，如果可以的话走webview的goBack，不能才关闭Activity。

```java
@Override
public void onBackPressed() {
  if (mWebView == null) {
    return;
  }
  if(mWebView.canGoBack()) {
    mWebView.goBack();
  } else {
    finish();
    // or
    // super.onBackPressed();
  }
}
```

##### webview不支持mailto:打开邮件客户端。

这个就是没有实现，需要客户端实现，大概在shouldOverrideUrlLoading这个回调里，检测url以mailto开头的，用Intent调起系统邮件应用。

##### webview不能选择文件

例如使用type=file的input，不能选择邮件。这个原因比较简单，就是没有实现。需要客户端实现，但是实现比较复杂，成本比较大，不同版本的Android还有些区别。最好和客户端的同学商量解决方案，一般我们是需要选择文件上传到服务器的话我们调一个客户端提供的接口，其他的由客户端处理，最后返回结果就好。好处是网页和客户端实现相对比较简单，坏处是每次不同的业务都需要如此实现一遍。不过也可以考虑客户端封装一个通用的文件上传接口，网页调用。

##### 关于关闭Activity，有某些情况可能会导致应用内存溢出，想起来的时候再补充。

##### 在webview里点击链接会跳到系统浏览器。

貌似设置一个新的WebviewClient就可以？

##### 修改user-agent

在websettings里面修改

```java
WebSettings settings = mWebView.getSettings();
settings.setUserAgentString("ua");
```

##### Android代码混淆问题。

之前跟客户端交互，用过addJavascriptInterface，这种方式，简单来说就是客户端给网页注入一个对象，这个对象有相应的方法可供js调用，正常用当然没问题，但是当客户端需要打一个代码混淆的包的时候，会把这个注入到网页的对象的方法名字也混淆掉，解决方法就是在混淆配置上设置不要混淆提供给网页的方法。

##### 一个音频网页，关闭后声音还在播。

这个只是我以前遇到的一个场景，由于当时我对Android还很陌生，也不知道原因和怎么解决。现在回忆一下猜测大概是Activity关闭的时候，wenview资源并没有释放，可能还需要调用webview本身的一些方法释放资源。下面的参考代码是网上找的，具体没有试过，仅作记录。

```java
mWebViewContainer.removeView(mWebView);
mWebView.removeAllViews();
mWebView.destroy();
```

##### Android 4.2一下版本的addJavascriptInterface漏洞问题。

#### ios wkwebview调用window.alert, window.prompt, window.confirm无效等问题。

需要客户端实现。

#### 如果跳转页面多，需要按多次返回键才能返回进入webview前一个页面。

可以让客户端在顶部栏加一个叉叉按钮，表明是关闭当前Activity的。

#### js与Android用addjavascriptInterface交互，当java定义的方法的参数类型与js实际传过来的参数类型不一致时，js报错。
Error: Method not found.

#### 执行字符串的js，如果传参为字符串，要注意不要再参数漏了单引号或者双引号，如果是数字则没问题。否则报missing ) after params list。
```java
@JavascriptInterface
public void callStringCallback(final String string) {
    Toast.makeText(mContext, "Call method: callStringCallback, params: " + string, Toast.LENGTH_LONG).show();
    new Handler(Looper.getMainLooper()).post(new Runnable() {
        @Override
        public void run() {
            String script = "window.onPgClientCallback('" + string + "')";

            if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.KITKAT) {
                mWebView.evaluateJavascript(script, null);
            } else {
                mWebView.loadUrl("javascript:" + script + "");
            }
        }
    });
}
```

##### 不知道如何调试ios wkwebview？用Safari Technology Preview
[Download Safari Technology Preview](https://developer.apple.com/safari/download/)

#### Android使用addJavascriptInterface的，假如配置代码混淆，注意不要把注入给js的方法名字混淆掉。

#### 关于localStorage

做内嵌于app的网页，真的经常会遇到不能用localStorage的情况，Android的要提醒客户端添加配置。

```java
settings.setJavaScriptEnabled(true);
```

#### 关于调试webview。

Android可以用chrome远程调试，同时客户端要添加配置。

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    WebView.setWebContentsDebuggingEnabled(true);
 }
```

或者在Android studio的logcat里面过滤chromium查看js打的log。

iOS则需要用Safari远程调试。如果普通的Safari不行，可以试试Safari technology preview。
