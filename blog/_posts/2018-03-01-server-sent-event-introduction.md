---
title: "Server Sent Event简介"
description: "Server Sent Event简介"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  - sse
tags:
  - sse
  - Server Sent Event
---

SSE早已不是什么新东西，不过真正使用的好像没见过。这里简单仅记录一些知识点。

Server-Sent Event简称SSE，是一种可以让服务器主动给网页发送消息的交互方式。这种技术适合一些只需要服务端给网页单向发送消息的场景。
服务端python事例代码（with tornado）：

```python
class IntervalHdl(tornado.web.RequestHandler):
    def initialize(self):
        pass

    def prepare(self):
        self.set_header('Content-Type', 'text/event-stream')
        self.set_header('Cache-Control', 'no-cache')
        self.set_header('Connection', 'keep-alive')
        self.set_header('X-Accel-Buffering', 'no')

    @tornado.gen.coroutine
    def get(self):
        count = 0
        lastid = self.request.headers.get('Last-Event-ID', 0)

        if lastid >= 5:
            self.set_status(204)
            raise self.finish()

        while True:
            count = count + 1
            event = 'pyevent'
            mod = round(random.random() * 10) % 2
            if mod == 0:
                event = ''

            self.write(
                'id:{}\nevent:{}\ndata:{}\n\n'.format(
                    count,
                    event,
                    json.dumps({
                        'error': mod,
                        'message': 'Hello %s' % datetime.datetime.now()
                    })
                )
            )

            if count >= 5:
                raise self.finish()

            self.flush()
            yield tornado.gen.sleep(5)
```

web前端应用的代码(with vue)：

```javascript
methods: {
        initEventSource() {
            const self = this;
            evtS = new window.EventSource(mCommon.util.getNamieApi('/api/sse/demon/interval/'));

            evtS.addEventListener('message', evt => self.handleData(evt), false);
            evtS.addEventListener('pyevent', evt => self.handleData(evt), false);
            evtS.addEventListener('error', evt => {
                console.log(evt);
                if (evt.target.readyState === EventSource.CLOSED) {
                    console.log('connection closed.');
                }
            }, false);
        },
        handleData(event) {
            console.log(event);
            // maybe check event.origin for security.
            let jsonStr = event.data;
            let obj = {};
            try {
                obj = JSON.parse(jsonStr);
            } catch (err) {
                obj.error = -1;
                obj.message = jsonStr;
            }
            this.message_list = [...this.message_list, {
                error: obj.error,
                message: obj.message,
                event_type: event.type
            }];
        },
        close() {
            evtS && evtS.close();
        }
    }
```

### Event Stream的格式
一组数据由几个固定的键值对组成：event, data, id, retry，键与值之间用冒号分隔。

不同的数据单元用两个换行符号分隔。

```text
event: event1
data: data1

event: event2
data: data2
data: data22
```

### 一些小细节

* http头。
* 返回的文本的格式。
* 是否支持跨域?当然支持。
* 连接断了浏览器自动重连。
* 一组数据以两个换行符分隔，一个事件可以有多个data，多个data会自动连接成一个字符串。
* 默认event type为message。
* Response status 需要是200.
* 假如连接断开，下次重连的时候，会在http头的Last-Event-ID带上上一次事件的id。
* 服务端关闭连接的办法，返回一个非200的status code，或者非text/event-stream的header。客户端关闭连接，js直接调用EventSource对象的close方法。

### 参考资料

> <site><a target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/API/EventSource">EventSource</a></site>
> <site><a target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events">Using server-sent events</a></site>
> <site><a target="_blank" href="https://www.html5rocks.com/en/tutorials/eventsource/basics/">Stream Updates with Server-Sent Events</a></site>
