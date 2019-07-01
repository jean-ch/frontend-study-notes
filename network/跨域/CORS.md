#### CORS (Cross-Origin-Resource-Sharing)
跨域资源共享

#### 和JSONP对比 
- 支持POST等所有HTTP请求 
- 安全性相对JSONP高
- 需要服务器支持
- 不兼容老版本浏览器

### 简单请求
#### 定义
- 请求方法局限于： 
  - HEAD
  - GET
  - POST
- HTTP头部信息字段局限于：
  - Accept
  - Accept-Language
  - Content-Language
  - Last-Event-ID
  - Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain

#### 请求  
- **Origin**   
说明来自哪个源（协议 + 域名 + 端口），服务器根据这个值，决定是否同意这次请求

#### 回应 
- **Access-Control-Allow-Origin**: 必须。它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求
```
response.setHeader("Allow-Control-Allow-Origin", "*")
```
- Access-Control-Allow-Credentials：true表示是否允许发送Cookie。默认Cookie不包括在CORS请求之中。  
  - 如果要发送cookie需满足
    - Access-Control-Allow-Credentials: true
    - 在AJAX请求中打开**withCredentials**属性  
      ```
      var xhr = new XMLHttpRequest();
      xhr.withCredentials = true;
      ```
    - Access-Control-Allow-Origin不能设为星号，必须指定明确的、与请求网页一致的域名
    - cookie遵循同源政策，只有用服务器域名设置的Cookie才会上传
  - 如果不再继续发送cookie：
    - 头部删除Access-Control-Allow-Credentials字段 
    - 显式关闭withCredentials  
    ```xhr.withCredentials = false;```
- Access-Control-Expose-Headers：可选。  
CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到以下6个基本字段，如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定  
  - Cache-Control
  - Content-Language
  - Content-Type
  - Expires
  - Last-Modified
  - Pragma   

### 非简单请求   
#### 预检请求 preflight
- 在正式通信之前，增加一次HTTP查询请求
- 用于询问服务器
  - 当前网页所在的域名是否在服务器的许可名单之中，
  - 可以使用哪些HTTP方法
  - 可以使用哪些头信息字段

####  预检请求的HTTP头信息
- **Origin**
- **Access-Control-Request-Method**：必须，列出浏览器的CORS请求会用到哪些HTTP方法
- Access-Control-Request-Headers：指定浏览器CORS请求会额外发送的头信息字段

####  预检请求的回应
- **Access-Control-Allow-Origin**
- **Access-Control-Allow-Methods**： 必需，表明服务器支持的所有跨域请求的方法（为了避免多次预检请求）
- Access-Control-Allow-Headers：表明服务器支持的所有头信息字段 
- Access-Control-Allow-Credentials
- Access-Control-Max-Age: 可选，指定本次预检请求的有效期，单位为秒

#### 正式请求
- Origin

#### 正式请求的回应
- Access-Control-Allow-Origin

### 代码  
```
function createCORSRequest(method, url) {
  var xhr = null;
  if ('withCredentials' in xhr) { // withCreadentials only exist in XMLHTTPRequest2
    xhr = new XMLHttpRequest();
    xhr.open(method, url, true); // async: true
  } else if (typeof XDomainRequest != 'undefined') { // XDomainRequest only exist in IE 
    xhr = new XDomainRequest();
    xhr.open(method, url);
  } 

  return xhr;
}

var xhr = createCORSRequest('GET', url);
if (!xhr) {
  throw new Error('CORS not supported');
}

xhr.onload = function() {
  var text = xhr.responseText;
  ...
}

xhr.onerror = function() {
  ...
}

xhr.send();
   
}
```
#### XDR- XDomainRequest
- 没有cookie
- 只能设置content-type
- 支支持Get， Post
- 不能访问response header
- open()只接受method和url，只支持异步请求

