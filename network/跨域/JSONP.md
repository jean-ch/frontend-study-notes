#### 和CORS对比
- 只支持GET请求
- 兼容老式浏览器
- 从其他域加载代码执行，如果其他域不安全，可能在相应中夹杂恶意代码
- 难以判断请求是否失败，只能依靠script元素的onerror

#### JSONP
通过动态创建script，再请求一个带参网址实现跨域通信
- 回调函数：在请求中指定
- 数据 （数据用JSON相似的格式传给回调函数）
- 动态script元素的src属性指定跨域url

```
<script>
    var script = document.createElement('script');
    script.type = 'text/javascript';

    // 传参一个回调函数名给后端，方便后端返回时执行这个在前端定义的回调函数
    script.src = 'http://www.domain2.com:8080/login?user=admin&callback=handleCallback';
    document.head.appendChild(script);

    // 回调执行函数
    function handleCallback(res) {
        alert(JSON.stringify(res));
    }
 </script>
```
服务端返回如下（返回时即执行全局函数）
```
handleCallback({"status": true, "user": "admin"})
```
nodejs代码示例
```
var querystring = require('querystring');
var http = require('http');
var server = http.createServer();

server.on('request', function(req, res) {
    var params = qs.parse(req.url.split('?')[1]);
    var fn = params.callback;

    // jsonp返回设置
    res.writeHead(200, { 'Content-Type': 'text/javascript' });
    res.write(fn + '(' + JSON.stringify(params) + ')');

    res.end();
});

server.listen('8080');
console.log('Server is running at port 8080...');
```

#### 应用场景
通常为了减轻web服务器的负载，我们把js、css，img等静态资源分离到另一台独立域名的服务器上，在html页面中再通过相应的标签从不同域名下加载静态资源，而被浏览器允许