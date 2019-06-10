- class的非现实定义在实例上的方法都定义在prototype上，而定义在this上的是定义在类的实例上   
- 属性名采用表达式  
```
let methodName = 'getArea';
class Square {
  constructor(length) {
    // ...
  }
  [methodName]() {
    // ...
  }
}
```
- class不存在变量提升   

#### super   
- super在方法中作为对象使用    
可以用来调用定义在父类prototype上的属性，不能用来访问定义在父类实例上的属性   
- super在static方法中
super指向父类，而普通方法中的super指向父类原型   
this指向子类，而普通方法中this指向子类实例  

#### 可以继承原生构造函数  
```class MyArray extends Array ```
