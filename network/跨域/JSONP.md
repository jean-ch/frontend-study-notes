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

##### 实现
- 创建 callback 方法。
- 插入 script 标签。
- 后台接受到请求，解析前端传过去的 callback 方法，返回该方法的调用，并且数据作为参数传入该方法。
- 前端执行服务端返回的方法调用。
```
function jsonp({url, params, callback}) {
    // 调用异步callback函数，用promise包
    return new Promise((resolve, reject) => {
        // 创建 script 标签
        let script = document.createElement('script');
        // 将回调函数挂在 window 上
        window[callback] = function(data) {
            resolve(data);
            // 代码执行后，删除插入的 script 标签
            document.body.removeChild(script);
        }
        // 回调函数加在请求地址上
        params = {...params, callback} //wb=b&callback=show
        let arrs = [];
        for(let key in params) {
            arrs.push(`${key}=${params[key]}`);
        }
        script.src = `${url}?${arrs.join('&')}`;
        document.body.appendChild(script);
    });
}
```
##### 使用
```
function show(data) {
    console.log(data);
}
jsonp({
    url: 'http://localhost:3000/show',
    params: {
        //code
    },
    callback: 'show'
}).then(data => {
    console.log(data);
});
```
##### 服务器端设置
```
    var params = qs.parse(req.url.split('?')[1]);
    var callback = params.callback;
    res.send(`${callback}('Hello!')`);
```
#### 应用场景
通常为了减轻web服务器的负载，我们把js、css，img等静态资源分离到另一台独立域名的服务器上，在html页面中再通过相应的标签从不同域名下加载静态资源，而被浏览器允许