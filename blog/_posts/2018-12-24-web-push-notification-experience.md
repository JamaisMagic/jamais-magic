---
title: "Web push notification experience"
description: "Web push notification experience"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - web push notification
  - service-worker
---

之前就想实践一下关于web推送相关的东西，感觉能做一些app可以做的事也是增强了web的功能和体验。早前只知道window.Notification可以做到给用户弹一个消息，但是对service worker则不太熟悉，这次整理一下一些基础点。

## 推送流程

流程大概是这样的：

1. 用户加载页面，download、install、and register service worker。
2. service worker监听push事件。
3. js脚本向用户请求推送权限。
4. 权限允许之后，就可以调用方法生成一个subscription，把subscription相关的信息发送到自己的服务端保存起来。
5. 需要向用户发送推送的时候，在自己的数据库查找需要发送的subscription，把相关的信息以及要发送的title、payload等向浏览器制定的push service发送请求。
6. 这样service worker就可以收到push事件，然后用Notification实例相关的方法显示消息。

## service worker

service worker我们用它来缓存静态文件，供离线用，这个应该不陌生了，这里主要还是谈push相关的东西。

首先，如果还是想方便地使用文件缓存功能，建议不要完全自己写这个文件，而是用google的workbox及相关的webpack插件，workbox已经封装好了缓存相关的逻辑，webpack插件也做了些提取缓存文件的url的工作，具体可以参考其文档。<a target="_blank" href="https://developers.google.com/web/tools/workbox/">Workbox</a>

用了Workbox之后，我们需要写的部分就好简单啦。我们只需要在service worker里监听push事件就能显示消息。

```javascript
self.addEventListener('push', event => {
  const payload = event.data ? event.data.text() : '';
  event.waitUntil(
    self.registration.showNotification('Message title', {
      body: payload,
    })
  );
});
```

同时，用户可能会点击推送消息，也可能关闭推送消息，所以也可以监听notificationclick和notificationclose事件。

```javascript
self.addEventListener('notificationclick', event => {
  console.log('notificationclick');
  event.notification.close();

  event.waitUntil(clients.matchAll({
    type: 'window'
    })
    .then(windowClients => {
      clients.openWindow('/');
    })
  )
});

self.addEventListener('notificationclose', event => {
  console.log('notificationclose');
});
```

以上代码为点击推送消息，打开首页。

## 订阅 subscription

service worker是用来接收和显示推送的，而接收推送是需要先订阅的，那么如何订阅呢？
答案在navigator.serviceWorker相关的属性和方法里。

当service worker注册成功后，会得到一个registration对象，利用registration.pushManager对象的subscribe方法则可以做推送的订阅。

```javascript
async function subscribe() {
  const registration = await navigator.serviceWorker.ready;
  let subscription = await registration.pushManager.getSubscription();

  if (!subscription || !subscription.endpoint) {
    const response = await fetch('your_vapid_api');
    const responseJson = await response.json();

    subscription = await registration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array(responseJson.data.publicKey)
    });
  }
}
```

上面的逻辑是先尝试获取已存在的subscription，没有的话，就做一个subscribe。
下面获取vapid的public key的接口需要服务端实现，urlBase64ToUint8Array这个函数也需要自己实现，具体可以Google。

另外需要注意的是，subscribe的时候，是需要向用户申请权限的，也就是说执行registration.pushManager.subscribe()这个方法的时候，浏览器会弹出权限申请框，如果用户不授权，就无法订阅。同时，根据我的试验，这个权限的获取和使用window.Notification.requestPermission()方法获取权限是相通的，一个推送权限，一个通知权限。
不一样的是，当我嫩调用方法获取当前权限状态的时候，window.Notification.permission返回的是granted，denied或者default三者其一，而registration.pushManager.getPermissionState()返回的则是granted、denied或prompt其中一个。

## vapid keys

做了订阅之后，理论上就可以发送推送了，不过不要着急，我们还有服务端相关的逻辑要处理。

上面已经提到，需要vapid，那么服务端生成一堆vapid的公私钥，生成的办法有很多，可以使用<a target="_blank" href="https://github.com/web-push-libs/web-push">web-push</a>这个库。这个库已经封装好了web push相关的一些功能，我们只需要调用它的方法即可。

```javascript
const fs = require('fs');
const util = require('util');
const webpush = require('web-push');

const writeFilePromise = util.promisify(fs.writeFile);

const vapidkeys = webpush.generateVAPIDKeys();
writeFilePromise('./vapid.json', JSON.stringify(vapidkeys))
  .then(() => console.log('done'))
  .catch(err => console.error(err));
```

## send notification

同样是用web push这个库。

```javascript
const result = await webPush.sendNotification(storedSubscriptionObject), 'payload', {
  TTL: ttl,
});
```

## notice

经过我的实践，有一些注意事项。

1. 在pc上，接收和显示推送需要浏览器是打开的（自己的网页无需打开），而在Android手机上，如果浏览器可以在后台自动启动，则进程被杀掉也能收到通知，但是如果浏览器不允许后台自动启动，被删掉进程后就不能接收通知。
2. 苹果Safari不支持基于service worker的push，他们有一套自己的玩法，所以想要在Safari下搞推送，要另外实现一套逻辑。
3. 为了能更及时地更新service worker文件，最好不要设置http缓存喔。
4. 向push service发送请求，其url其实在subscription对象的endpoint属性里头，目前这个不能自己指定，也就是说只能用浏览器自己指定的push service。
5. Google chrome用的是fcm，由于众所周知的原因，Google的服务在国内可能会不能访问，虽然我自己前几日试过不用梯子也能接收推送，但是难说不知道什么时候就不能用。


Referrers:

* <site><a target="_blank" href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa">@vue/cli-plugin-pwa</a></site>

* <site><a target="_blank" href="https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin">Workbox webpack Plugins</a></site>

* <site><a target="_blank" href="https://gist.github.com/malko/ff77f0af005f684c44639e4061fa8019">malko/urlBase64ToUint8Array.js</a></site>

* <site><a target="_blank" href="https://developers.google.com/web/fundamentals/push-notifications/how-push-works">How Push Works</a></site>

* <site><a target="_blank" href="https://web-push-book.gauntface.com/chapter-05/04-common-notification-patterns/">Common Notification Patterns</a></site>

* <site><a target="_blank" href="https://developers.google.com/web/fundamentals/push-notifications/subscribing-a-user#permissions_and_subscribe">Subscribing a User</a></site>
