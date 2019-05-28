#### Javascript为什么是单线程 
作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？  

#### synchronous, asynchronous 
synchronous: 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务 
asynchronous: 不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行   

#### 异步运行机制 
- 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）    
- 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件    
- 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行   
- 主线程不断重复上面的第三步    

### Event Loop 
##### setTimeOut(fn,0)
```
setTimeout(function(){console.log(1);}, 0);
console.log(2);
// output 2, 1
``` 
setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，也就是说，尽可能早得执行。它在"任务队列"的尾部添加一个事件，因此要等到同步任务和"任务队列"现有的事件都处理完，才会得到执行   

##### process.nextTick(node)  
在当前"执行栈"的尾部----下一次Event Loop（主线程读取"任务队列"）之前----触发回调函数。也就是说，它指定的任务总是发生在所有异步任务之前  

##### setImmediate(node)  
当前"任务队列"的尾部添加事件，也就是说，它指定的任务总是在下一次Event Loop时执行，这与setTimeout(fn, 0)很像  
需要多次event loop才能执行完
setTimeOut(fn,0)和setImmediate都是放在当前任务队列的尾部，哪个先执行呢？答案是不确定 
[代码示例](http://www.ruanyifeng.com/blog/2014/10/event-loop.html) 

##### promise和event loop  
```
console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
}, 0);
Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});
console.log('script end');
/***
script start
script end
promise1 // promise先放入任务队列
promise2
setTimeout // setTimeout在当前任务队列的尾端执行
***/
```