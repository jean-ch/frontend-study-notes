#### cookie 
- 浏览器：document.cookie
- 服务器：response header中的Set-Cookie
- cookie按域名分类存储     

浏览器用来记录用户身份
- 保存在浏览器端（session是保存在服务器端）
- 会话cookie： 不设置过期时间，存储在内存(memory)当中，关闭浏览器窗口cookie就消失
- 如果设置了过期时间，浏览器就把cookie存在硬盘(disk)中，只有过期时间到了，cookie才会被清除。存储在硬盘上的cookie可以在多个不同的浏览器进程之间共享  
- cookie存储大概只有4kb，不适合存储大量的信息

cookie的设置及发送过程 
- 客户端发送一个http请求到服务器端
- 服务器端发送一个http响应到客户端，其中包含Set-Cookie头部
- 客户端发送一个http请求到服务器端，其中包含Cookie头部
- 服务器端发送一个http响应到客户端

cookie的属性 
- name, value
- 有效期：Expires指定具体到期时间 maxAge以秒为单位  
默认值为-1，表示cookie保存在浏览器中，关闭浏览器将丢失 
0表示即时删除这个cookie
- domain + path指定浏览器发出 HTTP 请求时，哪些域名要附带这个 Cookie   
如果没有指定该属性，浏览器会默认将其设为当前 URL 的一级域名， 比如www.example.com会设为example.com
```
Cookie cookie = new Cookie("username","helloweenvsfei"); // 新建Cookie
cookie.setMaxAge(Integer.MAX_VALUE); // 设置生命周期为MAX_VALUE
response.addCookie(cookie); // 输出到客户端
```

#### session
- 保存在服务器端 (cookie是保存在浏览器端)
- 服务器端存储量没有限制，但是存储的多对服务端是有一定的压力的     

当需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否已包含了一个session标识（称为session id），如果已包含则说明以前已经为此客户端创建过session，服务器就按照session id把这个session检索出来使用（检索不到，会新建一个）    
如果客户端请求不包含session id，则为此客户端创建一个session并且生成一个与此session相关联的session id，session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个session id将被在本次响应中返回给客户端保存。
##### 保存sessionid的方法 
- cookie。但cookie可以人为禁止，需要有其他机制在cookie被禁时仍然把sessionid传给服务器
- URL重写： 把session id直接附加在URL路径的后面  
- 表单隐藏字段：服务器会自动修改表单，添加一个隐藏字段，以便在表单提交时能够把session id传递回服务器

#### cookie和session的区别
- 存取方式的不同
  - Cookie中只能保管ASCII字符串，其他内容需要编码
  - Session中能够存取任何类型的数据
- 隐私策略的不同
  - Cookie存储在客户端阅读器中，对客户端是可见的。客户端的一些程序可能会窥探、复制以至修正Cookie中的内容  
  假如选用Cookie，敏感的信息如账号密码等尽量不要写到Cookie中
  - Session存储在服务器上，对客户端是透明的，不存在敏感信息泄露的风险
- 有效期上的不同  
  - cookie的过期时间可以设的很大 
  - Session依赖于名为JSESSIONID的Cookie，而Cookie JSESSIONID的过期时间默许为–1，只需关闭了阅读器该Session就会失效，因而Session不能完成信息永世有效的效果。运用URL地址重写也不能完成。而且假如设置Session的超时时间过长，服务器累计的Session就会越多，越容易招致内存溢出
- 服务器压力的不同
  - Session是保管在服务器端的，每个用户都会产生一个Session。假如并发访问的用户十分多，会产生十分多的Session，耗费大量的内存
  - 而Cookie保管在客户端，不占用服务器资源。假如并发阅读的用户十分多，Cookie是很好的选择
- 跨域
  - Cookie支持跨域名访问，例如将domain属性设置为“.biaodianfu.com”，则以“.biaodianfu.com”为后缀的一切域名均能够访问该Cookie
  - Session不支持跨域名访问

#### webStorage- sessionStorage, localStorage 
localStorage HTML5本地存储web storage特性的API之一，用于将大量数据（最大5M）保存在浏览器中，保存后数据永远存在不会失效过期，除非用 js手动清除。     
不参与网络传输。     
一般用于性能优化，可以保存图片、js、css、html 模板、大量数据。  
```windows.sessionStorage```   
- localStorage: 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除
- sessionStorage: 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据
- 存储量是5mb，大于cookie的4kb存储量
- webStorage API
  - setItem (key, value)
  - getItem (key) 
  - removeItem (key) 
  - clear () 
  - key (index)     