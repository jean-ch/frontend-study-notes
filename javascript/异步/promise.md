Promise解决了多层callback传统写法下层叠套用的问题，通过维护状态(pending, resolved, rejected), 传递状态，利用promise提供的then, all等接口来实现链式调用   

#### 创建promise
- new Promise(promise1), return promise1
- Promise.resolve(promise1), return promise1
   
#### promise写法  
- resolve方法的作用是把pending变成resolved，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去        
- reject方法的作用是把pending变成rejected， 在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去  
- resolve和reject之后代码还会继续执行，then和catch中的callback在本轮事件循环结束时执行         
- then可以接收两个参数，第一个参数在resolved下调用，第二个在rejected下调用（可省略）   
- catch用来捕获错误（发生错误时promise的状态由pending变成rejected，调用catch），promise和then发生的错误都会被catch 
- promise和catch中抛出的错误外层没有catch会被promise吃掉，抛出错误后报错，但是外部代码还是会继续执行    
- then的第二个func参数和catch的区别：在then的第一个func参数中抛出的错误第二个参数捕捉不到，也就是then的第二个参数只捕捉promise抛出的错误    
- **finally**不接受任何参数，因此是状态无关的  
- Promise.resolve()方法用来包装一个Promise对象  
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

#### 串行执行一系列promise  
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
 
#### 用promise封装一个ajax 
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

#### 并行执行一系列promise 
##### Promise.all()- 多个任务并行执行 
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

##### Promise.race()- 容错，多个任务返回最先结果即可
```
Promise.race([p1, p2]).then(function(result) {
	console.log(result);	
});
```

#### interview questions  
##### then传非函数造成值穿透  
```
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log);
// 答案：1
``` 
1
- then和catch期望的都是一个函数。当接收的不是函数而是值的时候会发生值穿透
- 于是1会一路传给最后一个then，相当于console.log(1)
##### 叠用promise时的执行顺序    
```
new Promise(resolve => { // p1
    resolve(1); // 为什么1在2后面打印呢： 因为p1走到这就resolve了，但是它不会马上调到他的resolve方法上面去执行，它仍然会
    
    // p2
    Promise.resolve().then(() => {
      console.log(2); // t1
    });

    console.log(4)
}).then(t => {
  console.log(t); // t2
});

console.log(3);
```  
4 3 2 1
- 4和3是同步任务，按顺序输出
- p1, p2都到达resolve状态，按代码顺序先将t1放入microtask队列，再执行p1的resolve函数，将t2放入microtask队列。因此先输出2在输出1

```
Promise.reject('a')
.then(()=>{  
	console.log('a passed'); 
})
.catch(()=>{  
	console.log('a failed'); 
});  
Promise.reject('b')
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
```
b failed, a failed, b passed

##### 红灯3秒亮一次，绿灯1秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯
```
function Light(color) {
    this.color = color;
}

Light.prototype.on = function() {
    console.log(this.color);
}

let red = new Light('red');
let green = new Light('green');
let yellow = new Light('yellow');

function switchLight(timmer, light) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            light.on();
            resolve();
        }, timmer);
    });
}

function step() {
    Promise.resolve().then(() => {
        return switchLight(3000, red);
    }).then(() => {
        return switchLight(1000, green);
    }).then(() => {
        return switchLight(2000, yellow);
    })
}
```

##### 实现mergePromise函数，把传进去的数组按顺序先后执行，并且把返回的数据先后放到数组data中
```
const timeout = ms => new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve();
    }, ms);
});

const ajax1 = () => timeout(2000).then(() => {
    console.log('1');
    return 1;
});

const ajax2 = () => timeout(1000).then(() => {
    console.log('2');
    return 2;
});

const ajax3 = () => timeout(2000).then(() => {
    console.log('3');
    return 3;
});

const mergePromise = ajaxArray => {
    // 在这里实现你的代码
    // 保存数组中的函数执行后的结果
	// 要求分别输出
	// 1
	// 2
	// 3
	// done
	// [1, 2, 3]
	var data = [];
	var sequence = Promise.resolve(); // 维护一个promise
	ajaxArray.forEach(item => {
		sequence = sequence // 更新promise
		.then(item) // 第一次then用来执行数组中的函数
		.then((res) => {
			data.push(res);
			return data;
		});
	})
};

mergePromise([ajax1, ajax2, ajax3]).then(data => {
    console.log('done');
    console.log(data); // data 为 [1, 2, 3]
});
```

[Promise 面试题](https://segmentfault.com/a/1190000016848192)