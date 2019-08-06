##### then里面return一个始终不resolve也不reject的promise
```Promise.stop = function() { return Promise(function(){}; )}```   
```.then(value => { if() { return Promise.stop(); }})```
```
Promise.stop = function() {
  // 这个promise没有resolve也没有reject方法，因此既不会resolve也不会reject
  return new Promise(function(){})
}

doSth()
.then(value => {
  if (sthBigErrorOccured()) {
    // then里面return永远pending的promise，当前promise要等待这个promise的状态改变，因此后面的then,catch永远不会执行
    return Promise.stop()
  }
  // normal logic
})
.catch(reason => {// will never get called
  // normal logic
})
.then()
.catch(reason => {// will never get called
  // normal logic
})
.then()
.catch(reason => {// will never get called
  // normal logic
})
```
缺陷：造成内存泄漏，后面所有回调函数的内存都一直不能释放 

##### 返回特定的error，后面的catch进行判断之后层层reject出去，这样所有的then都不会执行
```.then(value => { if() { throw new Error('BIG_ERROR'); }}```   
```.catch(err => { if(err.message === 'BIG_ERROR') { throw err; }})```
```
doSth()
.then(value => {
  if (sthErrorOccured()) {
    throw new Error('BIG_ERROR')
  }
  // normal logic
})
.catch(reason => {
  if (reason.message === 'BIG_ERROR') {
    throw reason
  }
  // normal logic
})
.then()
.catch(reason => {
  if (reason.message === 'BIG_ERROR') {
    throw reason
  }
  // normal logic
})
.then()
.catch(reason => {
  if (reason.message === 'BIG_ERROR') {
    throw reason
  }
  // normal logic
})
```
缺陷：需要在每个catch里写判断语句

##### 用Promise.prototype.next封装上面的报错逻辑
报错直接在then里面return error值，值穿透传给下面的next
```
var BIG_ERROR = new Error('BIG_ERROR')

Promise.prototype.next = function(onResolved, onRejected) {
  return this.then(function(value) {
    if (value === BIG_ERROR) {
      return BIG_ERROR
    } else {
      return onResolved(value)
    }
  }, onRejected)
}

doSth()
.next(function(value) {
  if (sthBigErrorOccured()) {
    return BIG_ERROR
  }
  // normal logic
})
.next(value => {
  // will never get called
})
```
或者把Error用Promise.resolve封装成一个Promise
```
var STOP = {}
Promise.stop = function(){
  return Promise.resolve(STOP)
}

Promise.prototype.next = function(onResolved, onRejected) {
  return this.then(function(value) {
    if (value === STOP) {
      return STOP
    } else {
      return onResolved(value)
    }
  }, onRejected)
}

doSth()
.next(function(value) {
  if (sthBigErrorOccured()) {
    return Promise.stop()
  }
  // normal logic
})
.next(value => {
  // will never get called
})
```
