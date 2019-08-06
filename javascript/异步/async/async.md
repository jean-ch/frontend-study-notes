- async是generator的语法糖  
- async function返回Promise对象  
- await命令后面是个Promise对象  
- await返回的也是一个promise对象 
- async函数内部return语句返回的值是then方法回调函数的参数   
- async函数返回的Promise对象，必须等到内部所有await命令后面的Promise对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误    
- 把await放进try catch里，因为await后面的Promise可能rejected     
	```
	async function myFunction() {
		try {
			await somethingThatReturnsAPromise();
		} catch (err) {
			console.log(err);
		}
	}
	// or
	async function myFunction() {
		await somethingThatReturnsAPromise()
		.catch(function(err) {
			console.log(err);
		});
	}
	```   
- 多个await如果不存在继发操作让他们同时触发   
	```
	// 不要这样写，是继发操作    
	let foo = await getFoo();
	let bar = await getBar();

	// 写法一
	let [foo, bar] = await Promise.all([getFoo(), getBar()]);

	// 写法二
	let fooPromise = getFoo();
	let barPromise = getBar();
	let foo = await fooPromise;
	let bar = await barPromise;
	```

#### 一道经典的面试题：Promise和Async的执行顺序
```
async function async1(){
	console.log('async1 start')
	await async2()
	console.log('async1 end')
}
async function async2(){
	console.log('async2')
}
console.log('script start')
setTimeout(function(){
	console.log('setTimeout') 
},0)  
async1();
new Promise(function(resolve){
	console.log('promise1')
	resolve();
}).then(function(){
	console.log('promise2')
})
console.log('script end')
/** 
output: 
script start
async1 start
async2
promise1
script end
promise2
async1 end
setTimeout
*/
```
**为什么promise2在async1 end之前打印**
- 执行到async1中的await async2()时，先执行console.log('async2')， 然后sync2返回一个promise
- 这个返回的promise会放到任务队列中等待执行。await让出执行栈。继续往下执行
- 继续执行到了new Promise，先把console.log('promise1')打印出来。然后把resolve任务放入任务队列中等待执行
- 全部同步任务执行完之后。看向任务队列
- 先把async2返回的promise拿出来执行。这个promise有个默认resolve函数，传入的参数是undefined，把这个resolve任务放入任务队列
- 然后把promise的resolve任务拿出来，执行console.log('promise2')
- 任务队列的最后是执行默认async2返回的promise的默认resolve，它啥也没干，但是它让await async2()终于有了返回结果
- 于是最后执行console.log('async1 end')


为什么async1 end在promise2之后打印呢：  
前情提要： async函数中返回的是一个Promise对象，而await表达式执行返回的值是Promise对象的处理结果，也就是Promise的回调函数resolve的参数   
按照代码顺序，micro task队列中async1返回的Promise对象在下面这个new的Promise对象之前，所以先把async1中的Promise拿出来执行   

执行到await async2()的时候，先把async里面return的promise放进任务队列中等待执行   

async1中虽然没有写出来，但是它有一个resolve函数，只不过传得参数是undefined。**把这个默认的resolve放进micro task的末尾等待执行**  
然后把new出来的Promise对象的resolve拿出来执行，于是打印出了promise2   
继续把之前放进去的async1中Promise对象的默认resolve拿出来执行，这下async1中的Promise对象终于执行完了，于是终于论到了打印async1 end    
[详解指路](https://segmentfault.com/a/1190000015057278)

#### 另一道面试题： 考察aync让出执行栈
[指路](https://www.cnblogs.com/lpggo/p/8127604.html)