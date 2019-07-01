优点：
1. 解决加载缓慢的第三方内容如图标和广告等的加载问题
2. Security sandbox
3. 并行加载脚本

缺点：
1. iframe会阻塞主页面的Onload事件
2. 即时内容为空，加载也需要时间
3. 没有语意

###### 获取iframe内容
- iframeElement.**contentWindow**
- window.**frames**[iframe_name]

###### iframe中调用父元素 
- window.**parent**.document.getElementById(), window.parent.document.method()