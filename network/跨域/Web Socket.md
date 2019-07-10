HTML5新增的传输协议

##### HTTP的问题
- 单向： 通信只能由客户端发起
如果服务器有连续的状态变化，客户端要获知就非常麻烦。我们只能使用"轮询"：每隔一段时候，就发出一个询问，了解服务器有没有新的信息。最典型的场景就是聊天室
- stateless

#### Web Socket
- 双向平等对话: 服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息
- 没有同源限制，客户端可以与任意服务器通信
- 持久连接

#### API
- new WebSocket('ws://localhost:8080')必须是绝对url
- readyState
  - CONNECTING: 0
  - OPEN: 1
  - CLOSING: 2
  - CLOSED: 3
- onopen
- onclose
- onmessage
- onerror
- send(JSON.stringify(message)) 用于向服务器发送序列化的数据