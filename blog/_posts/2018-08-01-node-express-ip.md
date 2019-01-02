---
title: "Node express ip"
description: "Node express ip"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - express ip
---

今日处理一个需求，背景不方便说，反正是需要把用户的ip地址转换为地理信息。

网上搜了下，发现要么用第三方服务，要么用一些开源免费数据库自己搭服务。感觉都不太适合。第三方服务免费有限制或者收费，自己搭其实可以，不过没那么快，后来问了同事，最后用的部门其他组现有的服务。

然后需要处理一个遗留的小问题，就是获取ip地址的问题，之前在nginx设置了X-Forwarded-For，node设置了trust proxy，已经可以顺利获取客户端ip，但是假如客户端使用了代理，X-Forwarded-For拿到的第一个ip会是一个loopback或者private的ip地址，假如又多重代理，可能还会有多个这种ip。解决办法自然想到把X-Forwarded-For的ip列表逐个判断，拿第一个非loopback和private的ip地址作为真实ip。

具体的判断用的第三方库哈。写完后本来想直接赋值给express的req.ip，后来测的时候发现赋值不生效，Google了一下发现这个东西和req.ips都是是个getter，每次访问时都会读X-Forwarded-For，改不了，然后就自己设置一个属性赋值。

顺便放一下我用来判断ip的库。

* <site><a target="_blank" href="https://github.com/whitequark/ipaddr.js">ipaddr.js</a></site>

以及express中关于ip的源码。

* <site><a target="_blank" href="https://github.com/expressjs/express/blob/master/lib/request.js">express/lib/request.js</a></site>

包括protocol，secure，ip，ips，subdomains，path，hostname，host等等属性都是一个getter，而没有定义setter。
