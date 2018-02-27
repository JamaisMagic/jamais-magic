---
title: "静态资源的跨域问题"
excerpt_separator: "<!--more-->"
categories:
  - cors
tags:
  - cors
---

本来静态资源例如图片仅作普通的展示并不会遇到什么跨域问题，都是交给浏览器下载展示。但是当资源需要交给js处理的时候，跨域问题就出来了。

举个例子：用js把非本域的图片转成base64。

```javascript
function toBase64(imageUrl, isCrossOrigin) {
  let image = new Image();
  let canvas = document.createElement('canvas');
  let context = canvas.getContext('2d');

  if (isCrossOrigin) {
    image.setAttribute('crossorigin', 'anonymous');
  }
  image.onload = () => {
    let imageWidth = image.width;
    let imageHeight = image.height;

    canvas.width = imageWidth;
    canvas.height = imageHeight;
    context.drawImage(image, 0, 0, imageWidth, imageHeight);

    let result = null;
    try {
      result = canvas.toDataURL('image/gif');
    } catch (error) {
      this.error = error;
      console.error(error);
    }
    this.base64 = result;
    console.log(result);
  };

  image.src = imageUrl;
}
```

执行上面的代码会报错

```javascript
toBase64('https://static.picoluna.com/resource/image/loading_pacman.gif', false);
```

```text
DOMException: Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported.
```

解决办法是在跨域的图片的http header添加Access-Control-Allow-Origin相关的头，同时在请求的图片添加crossorigin=anonymous的属性（如上面代码）。

具体例子可以配合[这个](https://jamais.picoluna.com/m/subject/blog_sample/origin_of_static_resource/)页面查看