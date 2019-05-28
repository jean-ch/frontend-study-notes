#### Javascript为什么是单线程 
作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？  

#### synchronous, asynchronous 
synchronous: 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务 
asynchronous: 不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行   

#### Javascript concurrency model
- memory heap： 分配variable 内存  
- call stack： 函数调用
- queue(queue-lick): event loop   

#### 异步运行机制 
- 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）    
- 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件    
- 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行   
- 主线程不断重复上面的第三步    

#### setTimeOut(fn,0)
task queue尾部 
```
setTimeout(function(){console.log(1);}, 0);
console.log(2);
// output 2, 1
``` 
