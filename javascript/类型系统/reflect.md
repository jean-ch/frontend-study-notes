- 让Object操作都变成函数行为   
```name in obj``` -> ```Reflect.has(obj, name)```
- 与Proxy对象的方法一一对应    
不管Proxy怎么修改默认行为，Reflect上始终是默认行为

#### Reflect上的静态方法
- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)