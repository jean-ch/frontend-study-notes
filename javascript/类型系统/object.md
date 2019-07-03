#### API
- Object.defineProperty(obj, name, value)
- Object.defineProperties(obj, {name: value})
- Object.getOwnPropertyDescriptor(obj, name)

#### 创建对象的方法
##### 工厂模式
```
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(o.name);
    };
    
    return 0;
}

var person = createPerson('Greg', 27, 'Doctor');
```
无法知道一个对象的类型 
 
##### 构造函数模式 
```
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function() {
        alert(this.name);
    };
}

var person = new Person('Greg', 27, 'Doctor');
```
缺陷：每个person实例都包含一个不同的sayName Function实例

##### 原型模式
```
function Person() {}
Person.prototype.name = 'Greg';
Person.prototype.age = 27;
Person.prototype.job = 'Doctor';
Person.prototype.sayName = function() {
    alert(this.name);
};

var person = new Person();
```
解决：方法放在原型上被所有实例共享
缺陷：引用类型被所有实例共享

##### 组合使用构造函数和原型模式  
- 构造函数中放特有属性和引用类型的属性 
- 原型上放方法避免生成重复的方法实例
```
function Person(name, age, job) {
    this.name = name;
    this.age = age;  
    this.friends = ['Shelby', 'Court'];
}

Person.prototype = {
    constructor: Person,  // 用字面量重新定义原型，constructor要指回原来的constructor
    sayName: function() { 
        alert(this.name); 
    }
}
```