---
title: "关于node.js做文件上传"
description: "关于node.js做文件上传"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - node.js
  - file upload
---

文件上传一般用form data，http头的content type为multipart/form-data; boundary=---------------------------

之前用的http client是axios，用着觉得挺好的。但是它有个不好的地方就是在node端没有对form data做专门的处理，不过好在node有个包，叫form-data，可以配合axios一起用。

这个包添加数据本身和浏览器的form-data一样的，不多讲述，需要注意的是除了把数据放在请求body里面，还需要设置content-type，一个form-data的实例有个叫getHeaders的方法，用这个方法可以获取需要设置的请求头，千万不要自己写字符串multipart/form-data来设置，因为这个就没有了分界线，服务器解析不了。另外有的服务器可能还需要设置content-length，同样调form-data实例的getLength或者getLengthSync方法即可，显然，这个方法区分同步和异步。用fs.createReadStream读取的文件是不能用同步那个方法的喔。

另外我自己的实践是，前端上传文件到我的服务器，临时存在服务器的硬盘，然后再上传到上一级服务器，上传成功后把本地的文件删除，返回上一级服务器返回的文件url给前端。上一级服务器应该是传到亚马逊s3哈。

至于node接收文件，有个叫multer的库可以直接用，不过需要注意的是错误处理要自己处理好，也就是需要写个中间件（虽然它本身就是个中间件）手动调用multer，捕获错误，再处理，不然错误就到express默认的处理函数。
