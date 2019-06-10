应用： observable中传入的对象data change触发observe函数自动执行 
```
const person = observable({
  name: '张三',
  age: 20
});

function print() {
  console.log(`${person.name}, ${person.age}`)
}

observe(print);
person.name = '李四';
// 输出
// 李四, 20
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
//先定义一个set几个，将所有观察者函数放入这个集合
observable函数返回一个代理，拦截原始对象的set操作，每一个set都自动执行相对应的观察者
```