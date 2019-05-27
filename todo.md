- [scope & closure](https://juejin.im/post/5cb3376bf265da039c0543da#heading-2)  
- [call & apply](https://juejin.im/post/5cb3376bf265da039c0543da#heading-9)  
- tcp三次握手和四次挥手，拥塞控制  
- 网络协议与层模型  
- 画出SSL四次握手过程，中间有对称加密或非对称加密吗，优化这一层  
- 事件委托机制(事件捕获、冒泡，父元素监听)     
- [HTTP状态码(http://hpoenixf.com/%E9%9D%A2%E8%AF%95%E5%BF%85%E8%80%83%E4%B9%8Bhttp%E7%8A%B6%E6%80%81%E7%A0%81%E6%9C%89%E5%93%AA%E4%BA%9B.html)  
- [系统优化- CDN加速服务](https://www.zhihu.com/question/36514327)  
- let和var的区别，let存在变量提升吗  
- 简述一下用户访问网站的过程（缓存，DNS查询，建立链接，请求，响应，收到页面，解析DOM树，显示内容，首屏加载完成，可交互） 
- 列举数组的用法（建议分类列举，栈、队列、映射、删除等） 
- 跨域（图像ping， JSONP ， CORS ， webSocket 等） 
- 本地存储（ cookie ， localStorage ， sessionStorage 等）
- [进程通信，有名和匿名管道](https://blog.csdn.net/zm1_1zm/article/details/52987310)  
- CSS 的单行和多行截断？（ overflow ， text-overflow ）
- 自定义事件实现方法（参看红宝书）
- getter 和 setter 写法（参看红宝书）
- ajax  
```
//open()接受3个参数：请求类型、 URL 和是否异步的布尔值
//GET方式通常这样发：
xhr.open("get", "example.php?name1=value1&name2=value2", true)

//可以设定请求头，包括自定义请求头，比方说这样：
xhr.setRequestHeader("MyHeader", "MyValue")；

//可以这样取得一个包含所有头部信息的长字符串：
var myHeader = xhr.getResponseHeader("MyHeader");
var allHeaders = xhr.getAllResponseHeaders();

//POST方式有这几个地方要改：
//请求头要重设，数据会以key1=value1&key2=value2的方式发送到服务器
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
//获取表单
var form = document.getElementById("user-info");
//序列化表单，发送的内容传入send()
xhr.send(serialize(form));

//也可以这样传值：
var data = new FormData(form);
//再传一点别的
data.append("name", "Nicholas")
xhr.send(data);
```
- CSS 三栏布局，左右定宽，中间自适应（ flex ， grid 等） 
- 解释构造函数、对象、原型链之间的关系（看红宝书）  
- 手写代码，实现原型式继承（看红宝书） 
- 手写代码，实现借用构造函数（看红宝书） 
- absolute 依据的定位元素是？（非 static 的祖先元素） 
- parseInt() 和 array 的 map 方法的参数？（看红宝书） 
- absolute 依据的定位元素是？（非 static 的祖先元素） 
- webpack 用过吗？如何处理图片、 CSS 文件？（各种 loader ）
- HTML5 的语义化标签（ header ， footer ， main 等）
- z-index 的蜜汁用法（这是一个“拼爹”的属性） 
- 获取页面滚动高度（ window.pageYOffset ） 
- position 的取值和含义（W3Cschool-position属性） 
- 箭头函数、闭包、异步（老生常谈，参见上文） 
- 简述动画写法（ setTimeout ， requestAnimationFrame ， CSS3 ……）  
- Node 的多线程，高并发，安全  
- event loop （听过，不太会）  

- [Express和Koa的区别，优缺点](https://www.zhihu.com/question/38879363)  
- Node.js 的优缺点（擅长I/O密集、不擅长计算密集……） 
- Vue 的生存周期（创建，挂载，更新，销毁） 
