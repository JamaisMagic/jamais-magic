---
title: "web api request limitation"
description: "web api request limitation"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - api request limitation
---

之前写接口，就想做一些防刷的控制。但是没有很好的思路，做出来的是几多几多秒内只能请求一次，这样的效果。

今日和服务端的同事聊了下，发现可以用计数的方式，做几多几多秒内请求几多几多次。

顿时阔然开朗。

大概思路是，用户有请求过来时，根据一个id（用户id或者ip地址之类的，或者自定义的业务字段），记录这个id的请求次数，请求次数怎么记呢，就是利用redis的incr命令，这个命令是假如没有值，设为0，假如有值，则加1，当取到值为null或者0时用expire命令设置过期时间为自定义的时间（例如60秒）。这样，判断大于某个数字染回错误码给客户端即可。设定的时间过去后，redis记录清空，重新计算。

具体代码逻辑大概可以参考这里，作为一个中间件使用。这个代码只是做逻辑参考，不能直接copy paste运行的喔。

```javascript
module.exports = function(limitCount=30, limitTime=60) {
    return async (req, res, next) => {
        const redis = req.app.get('connRedis');

        if (!redis) {
            return res.send('Catch server error.');
        }

        let ip = req.ip;
        let key = `limiter_ip_${ip}`;
        let count = await redis.get(key);

        if (count >= limitCount) {
            return res.send('Request toot frequently');
        }

        redis.incr(key);

        if (!count) {
            redis.expire(key, limitTime);
        }

        next();
    };
};
```

* <site><a target="_blank" href="https://redis.io/commands/incr">INCR key</a></site>

* <site><a target="_blank" href="https://redis.io/commands/expire">EXPIRE key seconds</a></site>
