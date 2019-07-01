#### SSE (Server-Sent Event)
服务器向浏览器的单向通信，通过一个持久的HTTP相应发送，MIME类型是text/event-stream

#### 和Web Socket的区别
- SSE仍旧基于HTTP
- SSE自动支持断线重连
- SSE是服务器到浏览器的单项通信，XHR + SSE可以做到双向通信
- SSE也可以跨域

#### API
- new EventSource(url, { withCredentials: true }) 
    - 指定第二个参数支持跨域
    - withCredential true支持cookie
- onopen
- onmessage
- onerror
- 自定义事件
- source.close()

#### 数据格式
- header
    - Content-Type: text/event-stream
    - Cache-Control: no-cache
    - Connection: keep-alive
- message
    - 每一行都是[field]: value
    - field可取
        - data
        - event：可以自定义事件
        - id：支持同步机制，浏览器用Last-Event-ID读取这个id值，每次断线重连都将这个值发回给服务器帮助建立重新连接
        - retry：指定浏览器发起重新连接的时间间隔
