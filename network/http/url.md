#### url的格式
scheme :// host:port path ?query #fragment
- scheme: 协议名 
- host:port会被包括在在header的Host字段，因此发给服务器的时候会被浏览器省略
- path以/开头
- query由key=value&key=value组成
- fragment用于浏览器获取到资源后直接跳到指定的锚点位置 

#### url编码
- 转义 escape, unescape

#### 输入url后回车会发生什么
- 查找IP地址
  - 浏览器缓存中查找
  - 系统缓存中查找
  - hosts文件中查找（本地DNS）
  - 借助DNS域名解析查找
    - 本地DNS
    - 根DNS
    - 顶级DNS
    - 权威DNS
  - 可能给出CDN的服务器地址
    - CDN缓存了网站的大部分资源，例如图片，CSS样式表。有的HTTP请求CDN就可以直接响应
    - 对于CDN无法缓存的动态资源，如PHP JAVA等后台服务动态生成的页面，只能经过路由器，网关，代理到达目标服务器
  - 目标服务器内部有负载均衡
    - 首先访问memory级缓存Redis
    - 再访问disk级缓存Varnish
    - 最后访问应用服务器如Nodejs，和数据库
- 浏览器用三次握手与服务器建立连接
- 浏览器向服务器发送拼接好的报文
- 服务器收到报文后处理请求，同样拼接好报文发给浏览器
- 浏览器解析报文，渲染输出页面
- 没有四次挥手，因为长连接