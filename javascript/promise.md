Promise解决了多层callback传统写法下层叠套用的问题，通过维护状态(pending, resolveed, rejected), 传递状态，利用promise提供的then, all等接口来实现链式调用   
   
### promise写法  
- resolve方法的作用是把pending变成resolved，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去        
- reject方法的作用是把pending变成rejected， 在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去       
- then可以接收两个参数，第一个参数在resolved下调用，第二个在rejected下调用（可省略）       
- catch用来捕获错误（发生错误时promise的状态由pending变成rejected，调用catch），promise和then发生的错误都会被catch 
- **then的第二个func参数和catch的区别：**在then的第一个func参数中抛出的错误第二个参数捕捉不到，也就是then的第二个参数只捕捉promise抛出的错误    
```
new Promise(function(resolve, reject) {
	var timeOut = Math.random() * 2;
	setTimeOut(function(){ //在pormise里面包一个回调函数
			if (timeOut < 1) {
				resolve('200 OK');
			} else {
				reject('timeout in ' + timeOut + ' seconds.')
			}
		}, timeOut * 1000);
}).then(function(r) {
	log('Done: ' + r);
}).catch(function (reason) {
	log('Failed: ' + reason);	
})
```  
 
### 串行执行一系列promise  
- 能够实现链式写法的原因是在then和catch都返回一个新的Promise实例对象        
- 前面一个then完成后将return的结果作为参数传给第二个then  
- then的返回如何包装成Promise
	- 如果函数里面本来就return一个promise，要记得return，才能让下一个then基于这个return的promise的状态  
	- 如果return一个synchronized value，会被包装成promise， 这个synchromized value会被传参给第二个promise    
- 想要达到后面一个callback在前面一个callback状态resolve之后再调用，要用promise.all，而不是在callback上做forEach  
```
funtion multiply(input) {
	return new Promise(function(resolve, reject){
		setTimeOut(resolve, 500, input * input);  // pass input * input as parameter to the resolve function
	});		
}

funtion add(input) {
	return new Promise(function(resolve, reject) {
		setTimeOut(resolve, 500, input+ input);	// wait 0.5 second
	});
}

var promise = new Promise(funtion(resolve, reject)) {
	resolve(123);
}

promise.then(multiply)
       .then(add)
       .then(multiply)
       .then(add)
       .then(function(result) {
       		log("Got value: " + result);
   		});
```  
 
### 用promise封装一个ajax 
```
funtion ajax(method, url, data) {
	var request = new XMLHttpRequest();
	return new Promise(function(resolve, reject) {
		request.onreadystatuschange = function() {
			if (request.readyState == 4) {
				if (request.status == 200) {
					resolve(request.responseText);
				} else {
					reject(request.status);
				}
			}
		};		
		request.open(method, url);
		request.send(data);
	});
}

var promise = ajax('GET', url);
promise.then(function(text) {
	log.innerText = text;	
}).catch(function(status) {
	log.innerTexxt = "ERROR: " + text;
});
```  

### 并行执行一系列promise 
#### Promise.all()- 多个任务并行执行 
```
var p1 = new Promise(function(resolve, reject) {
	setTimeOut(resolve, 500, 'p1');
});

var p2 = new Promise(function(resolve, reject) {
	setTimeOut(resolve, 500, 'p2');	
});
Promise.all([p1, p2]).then(function(results) {
	console.log(results);
});
``` 

#### Promise.race()- 容错，多个任务返回最先结果即可
```
Promise.race([p1, p2]).then(function(result) {
	console.log(result);	
});
```

https://javascript.ruanyifeng.com/advanced/promise.html
http://es6.ruanyifeng.com/#docs/promise
https://developers.google.com/web/fundamentals/primers/promises