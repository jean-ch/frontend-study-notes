#### 和CORS对比
- 只支持GET请求
- 兼容老式浏览器
- 从其他域加载代码执行，如果其他域不安全，可能在相应中夹杂恶意代码
- 难以判断请求是否失败，只能依靠script元素的onerror

#### JSONP
- 回调函数：在请求中指定
- 数据 （数据用JSON相似的格式传给回调函数）
- 动态script元素的src属性指定跨域url
 
```
// 跨域服务器    
// 文件：flightResult.php  
callback({
    "code":"CA1998",
    "price": 1780,
    "tickets": 5
});
```

```
// 本地
<script type="text/javascript"> 
    // 得到航班信息查询结果后的回调函数 
    var flightHandler = function(data){
        alert('你查询的航班结果是：票价 ' + data.price + ' 元，' + '余票 ' + data.tickets + ' 张。');
    }; 
    // 提供jsonp服务的url地址，指定callback为flightHandler, 并传递code参数作为查询信息
    var url = "跨域服务器/flightResult.php?code=CA1998&callback=flightHandler";
    // 创建script标签，设置其src属性 
    var script = document.createElement('script'); 
    script.setAttribute('src', url); // script.src = url;
    // 把script标签加入head，此时调用开始 
    document.getElementsByTagName('head')[0].appendChild(script); 
</script>
```