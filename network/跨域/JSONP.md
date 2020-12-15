#### 和CORS对比
- 只支持GET请求
- 兼容老式浏览器
- 从其他域加载代码执行，如果其他域不安全，可能在相应中夹杂恶意代码
- 难以判断请求是否失败，只能依靠script元素的onerror

#### JSONP
- 简单来说是 script 可以实现跨域
- 那么我们在请求页面的 script 标签里面指定一个 callback function，它的参数是我们想要请求得到的数据
- 继而我们在页面上动态挂在一个 script，指定跨域的 url，并在 url 的参数里面指定我们要请求的 query params，和我们上面 script 里面已经写好的这个 callback function 的名字
- 于是我们就能实现 script 跨域，在请求的服务器里 query 到我们想要的数据，并且传给 callback function，我们就能够拿到 callback 之后的结果
- 为什么叫 JSONP 呢，因为我们在跨域服务器里 query 到的数据是类 JSON 格式的

##### 实现 example
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

[详细解释瞅这里](https://blog.csdn.net/hansexploration/article/details/80314948)