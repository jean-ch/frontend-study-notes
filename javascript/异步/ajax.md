### AJAX
- var xhr = new XMLHTTPRequest()
- xhr.open(method, url, true) 第三个参数是- 是否异步发送请求，默认是true。open并不是真正发送请求，而是启用一个请求准备发送
- xhr.send(content) 不需要请求主体的时候传入null
- 响应特性
	- responseText
	- responseXML: content-type是text/xml, application/xml时有这个
	- status
	- statusText
	- readyState 异步请求响应状态
		- 0：未初始化
		- 1：启动，已经调用open还未调用send
		- 2：发送，已经调用send还未收到响应
		- 3：接收，收到部分响应
		- 4：完成，收到全部响应
		- **onreadystatechange事件的callback必须在调用open()之前指定**
- xhr.abort() 取消异步请求
- setRequestHeader()**在open之后send之前调用**
- getResponseHeader(), getAllResponseHeaders()
##### create XMLHttpRequest object  
```
var xhr;
if (window.XMLHttpRequest) {
	xhr = new XMLHttpRequest(); // for IE7+, Firefox, Chrome, Opera, Safari
}
else {
	xhr = new ActiveXObject("Microsoft.XMLHTTP");
}
```  
##### set readystatechange event callback，必须在open()之前   
```
xhr.onreadystatechange = function() {
	if (request.readyState == 4) { 
		if ((request.status >= 200 && xhr < 300) || xhr == 304) {
			return success(request.responseText); 
		} else {
			return fail(request.status);
		}
	}
}
```  
##### open xhr   
```
request.open("GET, url, true); 
``` 
##### set request header，在open之后send之前
```
xhr.senRequestHeader("Content-type", "application/x-www-form-urlencoded");
```
##### send xhr
```
xhr.send(null)// Get请求没有content body, send传入null
xhr.send("name=" + UserName);//Post请求传入content body
```

#### GET
URL末尾的查询字符串必须经过encodeURIComponent()编码
```
function addURLParam(url, name, value) {
	url += (url.indexOf('?') == -1 ? '?' : '&');
	url += encodeURIComponent(name) + '=' + encodeURIComponent(value);
	return url;
}
```

#### Post
把表单数据序列化传给服务器```xhr.send(serialize(form))```

##### FormData
序列化表单。可以直接传给send方法
- var data = new FormData()
- data.apend(name, value)
- xhr.send(serialize(form)) -> xhr.send(new FormData(form))
