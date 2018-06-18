---
title: "Input type=file限制文件类型"
description: "Input type=file限制文件类型"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - input file
---

做文件上传嘛，就要用到input type=file啦。
需要限制文件类型嘛，自然想到accept这个属性啦。
根据mdn的官方文档，accept这个属性是支持两种类型的写法的，一种是mime type，例如accept=“image/jpeg”，
另一种是文件后缀，例如accept=“.jpg”。
经过我的实验，mime type这种写法不太好使，windows的chrome，accept="image/jpeg, image/jpg, image/png"不起作用，测试能选择webp的文件上传。然后我改成用文件后缀的写法。
另外，关于Android手机上忽略mime type弹出几个不相干的文件选择对话框的问题，可以参考这里。不过我试过自己的Android机都不支持。

* Android4.4不支持后缀名。
* Android4.4不支持类似capture=microphone的写法，7支持，可以默认弹出选择图片的对话框。
* macos也不支持类似capture=microphone的写法，而且会影响前面的media type过滤，导致过滤无效。
* ios safari不支持后缀名。


> <site><a target="_blank" href="https://www.wufoo.com/html5/accept-type-attribute/">The accept Attribute</a></site>

```html
<input type="file" accept="audio/*;capture=microphone">
<input type="file" accept="image/*;capture=camera">
<input type="file" accept="video/*;capture=camcorder">
```

> <site><a target="_blank" href="https://caniuse.com/#feat=input-file-accept">accept attribute for file input</a></site>
