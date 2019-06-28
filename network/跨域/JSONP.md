#### 和CORS对比
- 只支持GET请求
- 兼容老式浏览器

Web上使用script调用js文件不存在跨域问题（实际上，只要拥有src属性的标签都允许跨域，比如script,img,iframe）   
web端像调用脚本一样来跨域请求服务器上动态生成的js文件

跨域服务器    
文件：flightResult.php    
代码：   
```
flightHandler({
    "code":"CA1998",
    "price": 1780,
    "tickets": 5
});
```
本地

```
<script type="text/javascript"> 
    // 得到航班信息查询结果后的回调函数 
    var flightHandler = function(data){
        alert('你查询的航班结果是：票价 ' + data.price + ' 元，' + '余票 ' + data.tickets + ' 张。');
    }; 
    // 提供jsonp服务的url地址（不管是什么类型的地址，最终生成的返回值都是一段javascript代码） 
    var url = "跨域服务器/flightResult.php?code=CA1998&callback=flightHandler";
    // 创建script标签，设置其属性 
    var script = document.createElement('script'); 
    script.setAttribute('src', url); 
    // 把script标签加入head，此时调用开始 
    document.getElementsByTagName('head')[0].appendChild(script); 
</script>
```
这次我们做的是   
1. 动态创建脚本  
2. url中传递了一个code参数，服务器去做查询CA1998次航班的信息，callback参数告诉服务器，我的本地回调函数叫做flightHandler  
3. 跨域服务端调用这个函数flightHandler 页面将会弹出一个提示窗体。把票价、余票以及张数给传递回来了。  