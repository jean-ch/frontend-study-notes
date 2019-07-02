#### Prototype  
- 构造函数的prototype属性指向函数的原型对象- Person的prototype属性是指向Person prototype对象的指针
- 函数原型对象的constructor属性指向构造函数- Person prototype的constructor属性是指向Person的指针
- 函数实例的[[prototype]]/__proto属性指向函数的原型对象- person的[[prototype]]/__proto属性是指向Person prototype对象的指针
##### 确定原型链的方法
- isPrototypeOf  
```Person.prototype.isPrototypeOf(person) === true```
- Object.getPrototyoeOf
```Object.getPrototypeOf(person) == Person.prototype```
- obj.hasOwnProperty(name)检测属性存在于实例中
- instanceof 确定原型和实例的关系  
- 可以在实例上重定义（拦截）原型的上的方法和属性，也可以用delete删除实例上的拦截属性  
- for..in 遍历对象实例和方法中的可枚举属性  
- 结合in和hasOwnProperty可以确认属性在实例上还是在原型上
  ```
  funtion hasPrototypeProperty(object, name) {
      return !object.hasOwnProperty(name) && (name in object);
  } 
  ``` 
- Object.keys() 取得所有可枚举属性
    - Object.keys(obj) 取得实例上所有可枚举属性 
    - Object.keys(obj.prototype) 取得原型上所有可枚举属性 
    - 替代for..in
- Object.getOwnPropertyNames() 取得对象实例上所有属性，包括不可枚举    
  constructor这个不可枚举的属性也能被取到  

#### 继承  
- prototype chain: 把父类的实例赋给子类的原型，从而子类原型上[[prototpye]]指针指向父类的原型 
- constructor stealing: 在子类中调用父类的构造函数，从而解决两个问题： 
    - reference type应该在constructor中声明   
    - 能够通过子类的构造函数像父类构造函数传参 
```
function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
    alert(this.name);
};

function SubType(name, age) {
    SuperType.call(this, name); // constructor stealing: 在子类中调用父类的构造函数，从而解决两个问题： 1. reference type应该在constructor中声明， 2. 能够通过子类的构造函数像父类构造函数传参 
    this.age = age;
}

SubType.prototype = new SuperType(); // 把父类的实例赋给子类的原型，从而子类原型上[[prototpye]]指针指向父类的原型  
SubType.prototype.sayAge = funtion() {
    alert(this.name)
};
```  
- Object.create()实现原型继承，第一个参数是用作新对象原型的对象，第二个参数位新对象定义额外属性    
