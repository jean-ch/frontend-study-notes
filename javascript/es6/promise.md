Promise解决了多层callback传统写法下层叠套用的问题，通过维护状态(pending, resolveed, rejected), 传递状态，利用promise提供的then, all等接口来实现链式调用   
   
### promise写法  
- resolve方法的作用是把pending变成resolved，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去        
- reject方法的作用是把pending变成rejected， 在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去  
- resolve和reject之后代码还会继续执行，then和catch中的callback在本轮事件循环结束时执行         
- then可以接收两个参数，第一个参数在resolved下调用，第二个在rejected下调用（可省略）       
- catch用来捕获错误（发生错误时promise的状态由pending变成rejected，调用catch），promise和then发生的错误都会被catch 
- promise和catch中抛出的错误外层没有catch会被promise吃掉，抛出错误后报错，但是外部代码还是会继续执行    
- **then的第二个func参数和catch的区别：**在then的第一个func参数中抛出的错误第二个参数捕捉不到，也就是then的第二个参数只捕捉promise抛出的错误    
- **finally**不接受任何参数，因此是状态无关的  
- **Promise.resolve()**方法用来包装一个Promise对象  
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

### 几道题  
then传飞函数造成值穿透  
```
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log);
// 解析：p.then、.catch 的入参应该是函数，传入非函数则会发生值穿透；
// 答案：1
``` 
叠用promise时的执行顺序    
```
new Promise(resolve => { // p1
    resolve(1);
    
    // p2
    Promise.resolve().then(() => {
      console.log(2); // t1
    });

    console.log(4)
}).then(t => {
  console.log(t); // t2
});

console.log(3);
// 解析：
// 1. new Promise(fn), fn 立即执行，所以先输出 4；
// 2. p1和p2的Promise在执行then之前都已处于resolve状态
//    resolve(1)是立即执行的resolve，放在本轮时间循环的末尾执行
//    故按照then执行的先后顺序，将t1、t2放入microTask中等待执行；
// 3. 完成执行console.log(3)后，macroTask执行结束，然后microTask
//    中的任务t1、t2依次执行，所以输出3、2、1；
// 答案：
// 4 3 2 1
```  
```
Promise.reject('a')
  .then(()=>{  
    console.log('a passed'); 
  })
  .catch(()=>{  
    console.log('a failed'); 
  });  
Promise
  .reject('b')
  .catch(()=>{  
    console.log('b failed'); 
  })
  .then(()=>{  
    console.log('b passed');
  })

// 解析：p.then(fn)、p.catch(fn)中的fn都是异步执行，上述代码可理解为：
//       setTimeout(function(){
//             setTimeout(function(){
//                  console.log('a failed'); 
//             });  
//       });
//       setTimeout(function(){
//             console.log('b failed');
//
//             setTimeout(function(){
//                  console.log('b passed'); 
//             });
//       });
// 答案：b failed
//       a failed
//       b passed
```

https://javascript.ruanyifeng.com/advanced/promise.html
http://es6.ruanyifeng.com/#docs/promise
https://developers.google.com/web/fundamentals/primers/promises