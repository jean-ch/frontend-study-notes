https://luohaha.github.io/Chinese-uvbook/
#### nodejs结构   
1. node.js标准库，由js编写，包装使用js的apis  
2. node bindings: javascript通过node bindings调用底层C/C++   
3. V8, libuc..
- V8: Google推出的Javascript VM, 提供了JS在非浏览器端的运行环境  
- libuv： 为nodejs提供跨平台，线程池，事件池（event loop），异步I/O等能力  

#### nodejs是单线程的吗   

js的执行是单线程，I/O交给libuc，然后libuv在指定时刻回调
##### 如何实现异步，非阻塞I/O  
1. nodejs从js代码通过node bindings调用C/C++代码，传入参数和回调函数  
2. C/C++代码将参数和回调函数封装成一个请求对象传给libuv  
3. libuv中线程池处理这个请求
4. libuv的event loop来处理何时来调用回调函数     

##### libuv并发数  
libuv线程池默认大小是4   

#### libuv event loop  
1. event loop在内部维持多个事件队列，或者叫观察者watcher（比如时间队列，网络队列等），在watcher中注册回调函数    
2. 当事件发生时，事件转入pending状态，在下一次event loop拿出来执行   
3. libuv执行一个相当于while true的循环，不断的检查各个watcher上是否有需要处理的pending状态的时间，如果有按顺序去触发队列中保存的事件    
4. libuv event loop每次执行一个回调  
   
