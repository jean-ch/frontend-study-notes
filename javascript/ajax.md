### AJAX的原生写法  
```
// 1. create XMLHttpRequest object
var request;
if (window.XMLHttpRequest) {
	request = new XMLHttpRequest(); // for IE7+, Firefox, Chrome, Opera, Safari
}
else {
	request = new ActiveXObject("Microsoft.XMLHTTP");
}

// 2. register callback function
request.onreadystatechange = callback;

// 3. send request
// Get
request.open("GET, "url", true); // open的第三个参数指定是否要使用异步，default是true 
request.send();
// Post
request.open("POST, "AJAX", true);
request.senRequestHeader("Content-type", "application/x-www-form-urlencoded");
request.send("name=" + UserName);

// 4. set function callback
function callback() {
	// readyState 0- 请求未初始化, 1- 服务器连接已建立, 2- 请求已接收, 3- 请求处理中, 4- 请求已完成，相应就绪
	if (request.readyState == 4) { 
		if (request.status == 200) {
			return success(request.responseText); // 成功，通过responseText拿到响应的文本
		} else {
			return fail(request.status); // 失败，根据响应码判断失败原因
		}
	}
}

function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}
```