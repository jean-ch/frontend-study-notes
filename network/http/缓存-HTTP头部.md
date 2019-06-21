### HTTP中和缓存相关的header字段
##### 通用头部字段 
- Cache-Control
  - no-cache
  - no-store
  - max-age
  - min-fresh
  - max-stale
  - only-if-cached
  - Public
  - Private
- Pragma: HTTP1.0时的一流字段，当值为no-cache时强制验证缓存 
- Date: 创建保温的日期时间，启发式缓存阶段会用到  

##### 响应头部字段
- ETag: 服务器生成资源的唯一标识
- Vary: 代理服务器缓存的管理信息
- Age: 资源在缓存代理中存贮的时长(取决于max-age和s-maxage的大小)

##### 请求头部字段 
- If-Match:　条件请求，携带上一次请求中资源的ETag，服务器根据这个字段判断文件是否有新的修改　　
- If-None-Match: 和If-Match作用相反，服务器根据这个字段判断文件是否有新的修改
- If-Modified-Since: 比较资源前后两次访问最后的修改时间是否一致

##### 实体头部字段 
- Expires: 告知客户端资源缓存失效的绝对时间
- Last-Modified: 资源最后一次修改的时间

#### 强缓存 [Cache-Control + Expires]

### 其他头部字段 
##### General Header
- Request URL
- Request Method
- status code
- Remote Address: 路由地址 
- Cache-Control
- Pragma
- Connection
  - Close: 本次响应结束后断开连接  
  - KeepAlive: 保持连接  
  - Keep-Alive: 希望保持连接多少秒
- Date: 发送消息的时间， 缓存时会用到 

##### Request Header
- Accept, Accept-Charset, Accept-Encoding, Accept-Language: 告诉服务器自己接受的类型，字符集，编码方法(是否压缩), 语言
- Authorization: 当客户端接收到来自WEB服务器WWW-Authenticate响应时，该头部用来回应自己的身份验证信息给服务器   
- **Cookie**

##### Response Header

