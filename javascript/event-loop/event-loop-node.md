#### process.nextTick(node)  
当前execution stack尾部  

#### setImmediate(node)  
task queue尾部      

#### Node中的event loop  
- timer: setTimeout, setInterval
- I/O callbakcks: other callbacks except timer and close callback  
- idle, prepare阶段: 仅node内部使用 
- poll阶段: 获取新的I/O时间，适当的条件下node将阻塞在这里  
- check: setImmediate
- close: socket.on('close', callback)

#### micro task的执行时机 
micro task在以上阶段切换的中间执行

#### setTimeout和setImmediate哪个先执行  
setTimeout在timer阶段，setImmediate在check阶段  
event loop准备时间 > setTimeout最小毫秒数：setTimeout先执行  
event loop准备时间 < setTimeout最小毫秒数：setImmediate先执行  
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
- 把setImmediate1加入task队列 
- 把setTimeout2加入task队列 
- 执行setImmediate1，输出'setImmediate1', 把setTimeout1加入task队列 
- 执行先放进去的setTimeout2，输出'setTimeout2'。 把nextTick放入microtask队列，把setImmediate2放入task队列 
- **timer中还有一个任务**，执行setTimeout1, 输出'setTimeout1' 
- 切换阶段，检查microtask队列中有nextTick，输出'nextTick' 
- check阶段，执行setImmediate2, 输出'setImmediate2'  