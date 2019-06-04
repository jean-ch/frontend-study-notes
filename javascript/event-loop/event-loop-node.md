#### Node中的event loop  
- timer: setTimeout, setInterval
- I/O callbakcks: other callbacks except timer and close callback  
- idle, prepare阶段: 仅node内部使用 
- **poll阶段**: 执行I/O callback，适当的条件下node将阻塞在这里  
- check: setImmediate
- close: socket.on('close', callback)

#### micro task的执行时机 
micro task在以上阶段切换的中间执行

#### setTimeout和setImmediate哪个先执行- 不一定！  
setTimeout在timer阶段，setImmediate在check阶段  
到达poll阶段的时候有setImmediate优先执行setImmediate，没有再回去检查有没有到时间的setTimeout  
setTimeout的第二个参数是0，但实际做不到0       
所以取决于event loop的准备时间，准备时间小于setTimeout的最小等待时间就先执行setTimeout。反之如果大于，就先进入poll阶段，然后发现有setImmediate，优先执行setImmediate   
执行代码的时候timers的时间是随机的，所以setTimeout和setImmediate的执行顺序不确定   

```
setImmediate(() => { //setImmediate1
  console.log('setImmediate1')
  setTimeout(() => { //setTimeout1
    console.log('setTimeout1')
  }, 0);
})
setTimeout(()=>{ //setTimeout2
  process.nextTick(()=>console.log('nextTick'))
  console.log('setTimeout2')
  setImmediate(()=>{ //setImmediate2
    console.log('setImmediate2')
  })
},0);
```
假设先执行setImmediate
- 把setImmediate1加入task队列 
- 把setTimeout2加入task队列 
- 执行setImmediate1，***输出'setImmediate1'***, 把setTimeout1加入task队列 
- 执行先放进去的setTimeout2，***输出'setTimeout2'***。 把nextTick放入microtask队列，把setImmediate2放入task队列 
- 检查到nextTick，插入执行nextTick，***输出'nextTick'***     
- 执行setImmediate2, ***输出'setImmediate2'***    
- 执行setTimeout1, ***输出'setTimeout1'***   

##### io阶段setImmediate优先于setTimeout
```
var fs = require('fs')

fs.readFile(__filename, () => {
    setTimeout(() => {
        console.log('timeout');
    }, 0);
    setImmediate(() => {
        console.log('immediate');
    });
});
```
fs.readFile的回调是在poll阶段执行的  
到了poll阶段，setTimmediate总是优先  

##### timmer阶段结束后setImmediate优先于setTimeout
```
setTimeout(() => {
    setImmediate(() => {
        console.log('setImmediate');
    });
    setTimeout(() => {
        console.log('setTimeout');
    }, 0);
}, 0);
```
执行完外层的setTimeout后timer阶段就结束了。所以里层的setImmediate优先于setTimeout    

##### process.nextTick  
nextTick在单独的nextTick队列, 遇到nextTick就插入执行     