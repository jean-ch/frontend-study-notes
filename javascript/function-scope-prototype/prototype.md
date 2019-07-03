### Prototype  
- 构造函数的prototype属性指向函数的原型对象- Person的prototype属性是指向Person prototype对象的指针
- 函数原型对象的constructor属性指向构造函数- Person prototype的constructor属性是指向Person的指针
- 函数实例的[[prototype]]/__proto属性指向函数的原型对象- person的[[prototype]]/__proto属性是指向Person prototype对象的指针
#### 确定原型关系的方法
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

### 继承  
#### 原型链   
```SubType.prototype = new SuperType()```
    - 子类的[[prototpye]]/__proto属性指向父类的原型对象- SubType的[[prototype]]/__proto指向SuperType的prototype
    - Object是原型链的起点 
#### 继承方法
##### 原型链继承
```
function SuperType() {
    this.colors = ['red', 'blue'];
}
function SubType(){}
SubType.prototype = new SuperType();
```
缺陷：
- 引用类型colors从SuperType的实例对象继承成为SubType的原型对象。SubType的每个实例对象都共享这个引用类型
- 创建子类实例时无法向构造函数传参

##### 借用构造函数
```
function SuperType(name) {
    this.colors = ['red', 'blue'];
    this.name = name;
    this.sayName = function() {
        alert(this.name);
    }
}

function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}
```
解决：
- 构造函数传参
- SubType借调SuperType的构造函数，把引用类型放在实例对象上
问题：
- 函数复用

##### 组合继承
```
function SuperType(name) {
    this.colors = ['red', 'blue']; // 引用类型放实例上
    this.name = name; // 传参
}

SuperType.prototype.sayName = function() { // 复用的方法放原型上
    alert(this.name); 
};

function SubType(name, age) {
    SuperType.call(this, name); // 借用构造函数- 传参， 引用类型放实例上
    this.age = age;
}

SubType.prototype = new SuperType(); // 继承原型
SubType.constructor = SubType; // 把prototype整个替换之后要把constructor指回来
SubType.prototype.sayAge = function() {
    alert(this.age);
}
```

##### 原型式继承
Object.create()
```
var persion = {
    name: 'Nico',
    friends: ['Shelby', 'Court']
};

var anotherPerson  = Object.create(person, {
    name: 'Greg'
});
```
- 第一个参数：新对象的原型对象
- 第二个参数：额外属性    
