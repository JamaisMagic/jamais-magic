---
title: "关于vue nexttick的实现"
description: "关于vue nexttick的实现"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - vue
  - nextTick
---

今日看了下vue的些少源码，看到nextTick方法的实现。

./src/core/util/next-tick.js

重点看看这一段代码，大概是用setImmediate，MessageChannel和setTimeout实现macrotask，二microtask则是用Promise实现。

```javascript
let microTimerFunc
let macroTimerFunc
let useMacroTask = false

// Determine (macro) task defer implementation.
// Technically setImmediate should be the ideal choice, but it's only available
// in IE. The only polyfill that consistently queues the callback after all DOM
// events triggered in the same loop is by using MessageChannel.
/* istanbul ignore if */
if (typeof setImmediate !== 'undefined' && isNative(setImmediate)) {
  macroTimerFunc = () => {
    setImmediate(flushCallbacks)
  }
} else if (typeof MessageChannel !== 'undefined' && (
  isNative(MessageChannel) ||
  // PhantomJS
  MessageChannel.toString() === '[object MessageChannelConstructor]'
)) {
  const channel = new MessageChannel()
  const port = channel.port2
  channel.port1.onmessage = flushCallbacks
  macroTimerFunc = () => {
    port.postMessage(1)
  }
} else {
  /* istanbul ignore next */
  macroTimerFunc = () => {
    setTimeout(flushCallbacks, 0)
  }
}

// Determine microtask defer implementation.
/* istanbul ignore next, $flow-disable-line */
if (typeof Promise !== 'undefined' && isNative(Promise)) {
  const p = Promise.resolve()
  microTimerFunc = () => {
    p.then(flushCallbacks)
    // in problematic UIWebViews, Promise.then doesn't completely break, but
    // it can get stuck in a weird state where callbacks are pushed into the
    // microtask queue but the queue isn't being flushed, until the browser
    // needs to do some other work, e.g. handle a timer. Therefore we can
    // "force" the microtask queue to be flushed by adding an empty timer.
    if (isIOS) setTimeout(noop)
  }
} else {
  // fallback to macro
  microTimerFunc = macroTimerFunc
}
```

不过setImmediate这货只有ie11+和edge浏览器支持。

> <site><a target="_blank" href="https://caniuse.com/#feat=setimmediate">can i use setimmediate</a></site>

MessageChannel的支持度貌似比较好。

> <site><a target="_blank" href="https://caniuse.com/#search=MessageChannel">can i use MessageChannel</a></site>

关于setImmediate如何工作的有点难找，google一下很多都是node相关的内容，就是不知道node的setImmediate跟edge的是否一样。

> <site><a target="_blank" href="https://www.nczonline.net/blog/2013/07/09/the-case-for-setimmediate/">The case for setImmediate()</a></site>

> <site><a target="_blank" href="https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timers">timers-and-user-prompts</a></site>

> <site><a target="_blank" href="https://groups.google.com/a/chromium.org/forum/#!topic/blink-dev/Hn3GxRLXmR0">setImmediate?</a></site>

另外顺便提一下判断就是否浏览器原生方法的函数isNative其实是用native code这个字符串来判断的，不小心的话可能会有问题。

```javascript
/* istanbul ignore next */
export function isNative (Ctor: any): boolean {
  return typeof Ctor === 'function' && /native code/.test(Ctor.toString())
}
```
