#### import用法
```@import url(url_address)```

#### link和import的区别
link | import
--- | ---
link属于XHTML标签 | @import完全是CSS提供的一种方式
link标签除了可以加载CSS外，还可以做很多其它的事情，比如定义RSS，定义rel连接属性等 | @import就只能加载CSS
link引用的CSS会和整个页面一起加载 | @import引用的CSS会等到页面全部被下载完再被加载。网速慢时会出现闪烁
浏览器全支持 | @import只有在IE5以上的才能识别
使用javascript控制dom去改变样式的时候，只能使用link标签 | @import不是dom可以控制的