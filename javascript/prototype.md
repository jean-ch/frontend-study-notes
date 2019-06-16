#### Object
- 数据属性 
    - [[Configurable]]
    - [[Enumerable]]
    - [[Writable]]
    - [[Value]]
- 访问器属性 
    - [[Configurable]]
    - [[Enumerable]]
    - [[Get]]
    - [[Set]]
- Object.defineProperty()
    - 改变默认属性
    - 定义访问器属性 
      ```
      Object.defineProperty(book, "year", {
          get: function() {},
          set: function() {}
      });
      ```
- Object.defineProperties()
- Object.getOwnPropertyDescriptor() 取得属性的描述值 

#### Prototype  
- 原型对象包含一个指向构造函数的指针
- 实例包含一个指向原型对象的指针([[prototyp]], __proto\__)
- instanceOf 确定原型和实例的关系  
- isPrototypeOf() ```Person.isPrototypeOf(person1) === true``` 
- Object.getPrototypeOf() ```Object.getPrototypeOf(person1) == Person.prototype```
- 可以在实例上重定义（拦截）原型的上的方法和属性，也可以用delete删除实例上的拦截属性 
- hasOwnProperty() 检测属性是否存在与实例中  
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

#### 创建对象  
组合使用构造函数和原型模式  
- 构造函数中放特有属性和reference type的属性 
- 原型上放方法避免生成重复的方法实例
```
function Person(name, age, job) {
    this.name = name;
    this.age = age;  // 每个实例不同的特有属性，放在构造函数里 
    this.friends = ['Shelby', 'Court']; // reference type的属性，不能够放在原型上共享的，放在构造函数里
}

Person.prototype = {
    constructor: Person,  // 用字面量重新定义原型，constructor要指回原来的constructor
    sayName: function() { // 方法定义在圆形上，避免继承的过程中同样的方法有重复的实例
        alert(this.name); 
    }
}
```

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
