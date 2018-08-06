---
title: "Cloudfront http headers"
description: "Cloudfront http headers"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - cdn
  - amazon cloudfront
---

之前做一个东西，需要判断用户ip所在的国家（或者说地区）

然后用了通部门不同组的同学做的一个geoip服务。但是由于他们的服务器资源并没有很充足，所以让我注意一下并发，如果预计并发高的时候要提早告诉他们。

我心想，目前网站并发量很低，不用担心。但是说不定哪天推广一波就高呢。

然后又想，不如自己做一套？其实用网上开源的数据库应该是可以做的，但是自己做也是要申请资源，没必要一个部门重复浪费。

后来上网看了下，发现有的cdn通过http头可以告知后端这种信息。然后我就把请求头打印出来，看看都有哪些。

结果还是能发现惊喜的。

我们用的cdn是亚马逊的cloudfront，然后可以有这些cdn添加的头。

'cloudfront-forwarded-proto': 'https',

'cloudfront-is-desktop-viewer': 'false',

'cloudfront-is-mobile-viewer': 'true',

'cloudfront-is-smarttv-viewer': 'false',

'cloudfront-is-tablet-viewer': 'false',

'cloudfront-viewer-country': 'VN',

x-amz-cf-id

看看这些头其实都很实用，能知道用户通过什么协议访问网站，知道用户是桌面还是手机用户还是其他用户等。

还有一个就是刚好可以用到的国家（或者说地区）

最后有个x-amz-cf-id，不知道用来干嘛的，查一查亚马逊的文档，发现是用来唯一标记一个请求的，相当于request id之类的。

可以参考下面亚马逊的文档。

哈哈，得闲可以留意一下自己用的cdn厂商支持一些怎样的功能，然后好好利用起来。

> <site><a target="_blank" href="https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/RequestAndResponseBehaviorCustomOrigin.html#request-custom-headers-behavior">Request and Response Behavior for Custom Origins</a></site>

> <site><a target="_blank" href="https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/header-caching.html#header-caching-web-device">Caching Content Based on Request Headers</a></site>

> <site><a target="_blank" href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">ISO 3166-1 alpha-2</a></site>
