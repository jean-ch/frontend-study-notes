#### HTTP Header
##### 响应头部字段
- ETag: 服务器生成资源的唯一标识
- Vary: 代理服务器缓存的管理信息
- Age: 资源在缓存代理中存贮的时长(取决于max-age和s-maxage的大小)的修改　　
- If-None-Match: 和If-Match作用相反，服务器根据这个字段判断文件是否有新的修改

##### 通用头部字段
- Request URL
- Request Method
- status code
- Remote Address: 路由地址 
- Host: 请求的主机名
- Cache-Control: 告诉客户端如何处理缓存 
  - 请求指令
    - no-cache：强制再次验证
    - no-store：不缓存
    - **max-age**：缓存的时长，以秒为单位     
    **用于强缓存**
    - min-fresh：期望在指定时间内响应仍旧有效
    - only-if-cached：从缓存中获取 
  - 响应指令
    - public：任意一方（客户端，代理服务器等）都能缓存该资源
    - private：所有内容只有客户端可以缓存，Cache-Control的默认取值
    - no-cache：客户端缓存内容，但是是否使用缓存则需要经过协商缓存来验证决定
    - no-store：所有内容都不会被缓存，即不使用强制缓存，也不使用协商缓存
    - max-age
- Pragma: HTTP1.0时的遗留字段，当值为no-cache时强制验证缓存
- Connection
  - Close: 本次响应结束后断开连接  
  - KeepAlive: 保持连接  
  - Keep-Alive: 希望保持连接多少秒
- Date: 创建报文的日期时间，启发式缓存阶段会用到   

##### 请求头部字段
- Accept, Accept-Charset, Accept-Encoding, Accept-Language: 告诉服务器自己接受的类型，字符集，编码方法(是否压缩), 语言   
- Host: 浏览器要找的主机
- Refer：告诉页面是从哪个页面跳转来的   
常用语防止下载，盗链   
- User-agent: 向服务器发送浏览器的版本，系统，应用程序的信息  
- **Cookie**: 通过它可以在客户端保存用户状态，即使用户关闭浏览器也能继续保存   
告诉服务器关于 Session 的信息，存储让服务器辨识用户身份的信息   
- **Authorization**: 当客户端接收到来自WEB服务器WWW-Authenticate响应时，该头部用来回应自己的身份验证信息给服务器   
**对应401状态码**    
- Date: 浏览器发送数据请求的时间  
- **If-Modified-Since**: 比较资源前后两次访问最后的修改时间是否一致，以秒为单位     
**和响应头部的Last-Madified构成一对，用于协商缓存** 
- **If-Match**:　条件请求，携带上一次请求中资源的ETag，服务器根据这个字段判断文件是否有新   
**和响应头部的Etag构成一对，用于协商缓存**  

##### 响应头部字段 
- Content-length, Content-encoding, Content-type： 告诉浏览器会送的数据长度，压缩格式，类型   
- Content-Disposition： 告诉浏览器以下在的方式打开数据   
- Location: 配合302状态码，告诉浏览器应该去找谁   
- Age- 响应在缓存中存在了多久
- **Location**: **对应301, 302状态码**，给出重定向的地址，浏览器自动连接
- **Set-Cookie**: 服务器端用来设置http的Cookie，每个字段对应一个cookie
- **Date**- 响应被服务器创建的时间   
**和Last-Modified构成一对，用于启发式缓存**
- **Expires**: 告知客户端资源缓存失效的时间，服务器的绝对时间    
**用于强缓存**, -1， 0表示不缓存  
- **Last-Modified**: 资源最后一次修改的时间，以秒为单位     
**和请求头部的If-Madified-Since构成一对，用于协商缓存** 
**和Date构成一对，用于启发式缓存**
- **Etag**： 实体标签（Entity Tag）,只要资源改变，Etag就变化   
**和请求头部的If-Match构成一对，用于协商缓存**  

