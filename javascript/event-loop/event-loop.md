#### Javascript为什么是单线程 
作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？  

##### 单线程的javascript如何处理异步I/O, HTTP请求  
Node- 单线程异步非阻塞模式- 

#### synchronous, asynchronous 
synchronous: 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务 
asynchronous: 不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行   

#### Javascript concurrency model
- memory heap： 分配variable内存  
- call stack： 函数调用
- queue(queue-lick): event loop   

#### 异步运行机制 
- **同步任务**放在**调用栈**中等待**主线程**依次执行    
- **异步任务**有了运行结果，把注册的回调函数放入**任务队列**          
- 一旦调用栈中的所有同步任务执行完毕，系统就会读取任务队列 ，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行   
- 主线程不断重复上面的第三步    

#### 定时器
- setTimeOut()  
任务队列尾部，等待主线程和任务队列处理完之后执行     
- setInterval(): 和setTimeOut的区别是setInterval可以反复执行  
- process.nextTick(): node定时器   
当前execution stack尾部，主线程上任务处理完之后就执行，是所有定时器回调函数中最先执行的  
多层套用的nextTick也都顺序加到execution stack尾部，一次event loop执行完     
- setImmediate(): node定时器   
任务队列的尾部  
多层套用的nextTick要多次event loop才能执行完   

#### macro task & micro task  
- macro task: script全部代码，setTimeout, setInterval, setImmediate, I/O, UI rendering  
- micro task: process.nextTick(node), promise, mutation observer  

##### Promise.resolve()
放在micro task的尾部，因此在本轮event loop结束时执行 

