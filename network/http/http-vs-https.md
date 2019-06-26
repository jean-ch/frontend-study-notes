### HTTP  
HTTP(Hypertext Transfer Protocol)协议以明文方式发送内容，不提供任何方式的数据加密，如果攻击者截取了Web浏览器和网站服务器之间的传输报文，就可以直接读懂其中的信息，因此，HTTP协议不适合传输一些敏感信息，比如：信用卡号、密码等支付信息。  
HTTP连接最显著的特点是客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。从建立连接到关闭连接的过程称为“一次连接”。  

### HTTPS  
HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议  
HTTPS和HTTP的区别主要如下： 
1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。 
2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。 
3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。 
4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。 
[HTTPS的工作原理](https://juejin.im/entry/58d7635e5c497d0057fae036)
[SSL的三次握手](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)  
[HTTP, HTTPS, HTTP2应该知道的事](http://www.alloyteam.com/2016/07/httphttp2-0spdyhttps-reading-this-is-enough/)   
