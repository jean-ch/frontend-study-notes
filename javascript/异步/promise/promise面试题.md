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

#### 用promise和async实现每间隔1s,2s,3s...打印i
- 可以想到是一个promise数组，链式调用
  - 实现链式调用： ```sequence = sequence.then(func)```
  - 传给then的是个return promise的方法
- 为实现计时结束时console，因该把计时器包在一个promise里，而console方法在这个promise的then回调函数里
```
const timeout = ms => new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve();
	}, ms * 1000);
});

let promises = [];
for (let i = 0; i < 5; i++) {
	promises.push(() => timeout(i).then(() => {
		console.log(i, new Date());
	}));
}

let sequence = Promise.resolve();

promises.forEach((promise) => {
	sequence = sequence.then(promise);
});
```
- promise + async
  - 实现一个sleep函数
```
function sleep(ms) {
	return new Promise(resolve => {
		setTimeout(resolve, ms * 1000);
	});
}

async function count() {
	for (let i = 0; i < 5; i++) {
		await sleep(i);
		console.log(i, new Date());
	}
}
```

[Promise 面试题](https://segmentfault.com/a/1190000016848192)