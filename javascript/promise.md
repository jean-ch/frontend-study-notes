### promise写法  
```
new Promise(function(resolve, reject) {
	var timeOut = Math.random() * 2;
	setTimeOut(function(){
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
					reject(request.reponseText);
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