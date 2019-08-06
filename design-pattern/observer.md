- publisher维护一个subscriber callback function列表
- subscribe：往publisher的subscriber列表中添加一个callback
- unsubscribe：从publisher的subscriber列表中删除这个callback
- publish：调用subscriber列表中的每个callback

```
Publisher.prototype.publish = function(news) {
    var publisher = this;
    this.subscribers.forEach(subscriber => subscriber(news, publisher));
    return this;
}

Function.prototype.subscribe = function(publisher) {
    var subscriber = this;
    if (!publisher.subscribers.includes(item)) {
        publisher.subscribers.push(subscriber);
    }

    return this;
}

Function.prototype.unsubscribe = function(publiser) {
    var subscriber = this;
    publisher.subscribers.filter(item => item !== subscriber);
    return this;
}

```

#### 用proxy实现观察者模式   
```
const queuedObservers = new Set();

const observe = fn => queuedObservers.add(fn);
const observable = obj => new Proxy(obj, {set});

function set(target, key, value, receiver) {
  const result = Reflect.set(target, key, value, receiver);
  queuedObservers.forEach(observer => observer());
  return result;
}
//将所有观察者函数放入set
//observable函数返回一个代理，拦截原始对象的set操作，每一个set都自动执行相对应的观察者
```