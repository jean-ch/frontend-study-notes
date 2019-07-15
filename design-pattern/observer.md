```
Publisher.prototype.publish = function(news) {
    var publisher = this;
    this.subscribers.forEach(subscribe => subscribe(news, publisher));
    return this;
}

Function.prototype.subscribe = function(publish) {
    var subscriber = this;
    var exist = publish.subscribers.some(item => item === subscriber);
    if (!exist) {
        publish.subscribers.push(subscriber);
    }

    return this;
}

Function.prototype.unsubscribe = function(publish) {
    var subscriber = this;
    publish.subscribers.filter(item => item !== subscriber);
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