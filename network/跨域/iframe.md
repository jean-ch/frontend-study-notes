优点：
1. 解决加载缓慢的第三方内容如图标和广告等的加载问题
2. Security sandbox
3. 并行加载脚本

缺点：
1. iframe会阻塞主页面的Onload事件
2. 即时内容为空，加载也需要时间
3. 没有语意

#### iframe用法
##### 获取iframe内容
- iframeElement.**contentWindow**
- window.**frames**[iframe_name]

##### iframe中调用父元素 
- window.**parent**.document.getElementById(), window.parent.document.method()

#### document.domain + iframe跨域
仅限主域相同，子域不同的跨域应用场景   
两个页面都通过js强制设置**document.domain**为**相同的基础主域**，从而实现同域    
子页面能够直接通过js访问父页面的任何对象   

父窗口 http://www.domain.com/a.html
```
<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = 'domain.com';
    var user = 'admin';
</script>
```
子窗口 http://child.domain.com/b.html
```
<script>
    document.domain = 'domain.com';
    // 获取父窗口中变量
    alert('get js data from parent ---> ' + window.parent.user);
</script>
```

#### location.hash + iframe
location.hash是url里的anchor值(#...=...)  
改变location.hash不会导致页面刷新   
但是数据容量有限     

A与B相互通信，通过中间页C来实现:
  - A创建一个隐藏iframe，src指向B
  - 将hash值作为参数传递使用
  - B通过修改A传过来的hash值来传递数据
  - C是B的子页面，通过location.hash传值
  - C与A同域，通过js来通信(window.parent.parent访问A页面所有对象)，把数据结果传回去  

A http://www.domain1.com/a.html
```
<iframe id="iframe" src="http://www.domain2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 向b.html传hash值
    setTimeout(function() {
      iframe.src = iframe.src + '#user=admin';
    }, 1000);
    
    // 开放给同域c.html的回调方法
    function onCallback(res) {
      alert('data from c.html ---> ' + res);
    }
</script>
```
B http://www.domain2.com/b.html
```
<iframe id="iframe" src="http://www.domain1.com/c.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 监听a.html传来的hash值，再传给c.html
    window.onhashchange = function () {
      // 执行一些操作
      iframe.src = iframe.src + location.hash; 
    };
</script>
```
C http://www.domain1.com/c.html
```
<script>
    // 监听b.html传来的hash值
    window.onhashchange = function () {
        // 再通过操作同域a.html的js回调，将结果传回
        window.parent.parent.onCallback('hello: ' + location.hash.replace('#user=', ''));
    };
</script>
```

#### window.name + iframe
- iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。巧妙地绕过了浏览器的跨域访问限制，但同时它又是安全操作
- name值在不同的页面（甚至不同域名）加载后依旧存在
- name支持很长的值

父页面 http://www.domain1.com/a.html
```
var proxy = function(url, callback) {
    var state = 0;
    var iframe = document.createElement('iframe');

    // 加载跨域页面B
    iframe.src = url;

    // onload事件会触发2次，第1次加载跨域页，并留存数据于window.name
    iframe.onload = function() {
        if (state === 1) {
            // 第2次onload(同域proxy页)成功后，读取同域window.name中数据
            callback(iframe.contentWindow.name);
            destoryFrame();

        } else if (state === 0) {
            // 第1次onload(跨域页)成功后，切换到同域代理页面
            iframe.contentWindow.location = 'http://www.domain1.com/proxy.html';
            state = 1;
        }
    };

    document.body.appendChild(iframe);

    // 获取数据以后销毁这个iframe，释放内存；这也保证了安全（不被其他域frame js访问）
    function destoryFrame() {
        iframe.contentWindow.document.write('');
        iframe.contentWindow.close();
        document.body.removeChild(iframe);
    }
};


// 请求跨域b页面数据
proxy('http://www.domain2.com/b.html', function(data){
    alert(data);
});
```

中间代理页，与a.html同域，内容为空即可：http://www.domain1.com/proxy....      

子页面 http://www.domain2.com/b.html
```
<script>
    window.name = 'This is domain2 data!'; // 子页面的window.name上放要传输的数据
</script>
```