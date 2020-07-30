### API
#### Object.is(obj1, obj2)
可以准确判断NaN ```Object.is(NaN, NaN);```

#### Object.assign(target, source1, source2...)
将source对象的所有可枚举属性赋给target
- 浅拷贝
- 只拷贝对象的实例属性，**不拷贝继承属性和不可枚举属性**  
- 不是对象会先转成对象   
- undefined和null无法转成对象，作为首参数会报错，否则会跳过  
- 同名属性后面的source替换前面的source
- source如果是数组，视作对象处理，属性名是对应的 index
    ```
	Object.assign([1, 2, 3], [4, 5])
	// [4, 5, 3] 4作为属性名为1的对象覆盖了前面的1
	```
- 应用：为对象添加属性，方法，属性默认值，克隆对象，合并多个对象
- 缺陷：无法正确拷贝get和set属性。Object.getOwnPropertyDescriptors() + Object.defineProperties()可以实现  
    ```
    const source = {
    set foo(value) {
        console.log(value);
    }
    };

    const target2 = {};
    Object.defineProperties(target2, Object.getOwnPropertyDescriptors(source));
    Object.getOwnPropertyDescriptor(target2, 'foo')
    ```

#### 遍历对象的属性  
- for...in...  
	循环遍历对象自身的和**继承**的**可枚举**属性， 不包括Symbol属性     
- Object.keys(obj)  
	返回数组，包含对象自身**非继承**的**可枚举**属性，不包括Symbol属性  
- Object.getOwnPropertyNames(obj)  
	返回数组，包含对象自身的属性，包括**不可枚举**的属性，但不包括Symbol  
- Object.getOwnPropertySymbols(obj)    
	返回数组包含对象自身所有的**Symbol**属性     
- Reflect.ownKeys(obj)   
	返回数组包含**所有**键名，包括Symbol和不可枚举属性    

#### 对象的原型链继承方法  
- Object.setPrototypeOf(obj, prototype)   
    - ```var o = new Obj() ==> Object.setPrototypeOf(o, Obj.prototype)```
    - 不再使用__proto
- Object.getPrototypeOf(obj)

#### 对象的属性方法
- Object.defineProperty(obj, name, value)
- Object.defineProperties(obj, {name: value})
- Object.getOwnPropertyDescriptor(obj, name)
- Object.getOwnPropertyDescriptors()可以拿到所有自身（非继承）属性，包括get，set

#### 对象的扩展运算符 
- 用于数组  
	```foo = { ...['a', 'b', 'c'] };// {0: "a", 1: "b", 2: "c"}```  
- 用于非对象  
	```{...1} // {} 等同于 {...Object(1)}, 先转成对象再解构 ```  
- 字符串  
	```{...'hello'} // {0: "h", 1: "e", 2: "l", 3: "l", 4: "o"}```   
- 对象的扩展运算符等同于Object.assign，**只拷贝对象的实例属性**，不能拷贝原型属性   
	```
	clone = {...a};
	clone = Object.assign({}, a);
	```
- 合并两个对象  
	```
	{...a, ...b}
	Object.assign({}, a, b)
	```
- 扩展运算符后面可以跟表达式

#### 解构赋值  
扩展运算符的解构赋值不能用于复制继承自原型对象的属性 
```
const o = Object.create({ x: 1, y: 2 }); //{a: 1, y: 2}是o的prototype
o.z = 3;
let { x, ...newObj } = o; //newObj无法取得y属性
let { y, z } = newObj;
x // 1
y // undefined
z // 3
```   

#### Iterator接口 
供for..of循环使用     
- Object.keys()    
	- 数组的key是数组数字对应的string  
	```
	var arr = [1, 2, 3]
	Object.keys(arr)
	// [ '0', '1', '2' ]
	```
- Object.values()  
- Object.entries() 
	- 用于把object转成map  
	```
	const obj = { foo: 'bar', baz: 42 };
	const map = new Map(Object.entries(obj));
	map // Map { foo: "bar", baz: 42 }
	```  
- Object.fromEntries()   
	- 把键值对还原成对象  
	```
	Object.fromEntries([
		['foo', 'bar'],
		['baz', 42]
	])
	// { foo: "bar", baz: 42 }
	```   
	- 将Map转成对象   
	```
	const map = new Map().set('foo', true).set('bar', false);
	Object.fromEntries(map)
	```     

### 对象的super关键字   
- this指向方法所在的object  
- super指向object的原型  
	- super只能用在对象的方法中，不能用在对象的属性，或对象方法里面的函数中   

### 创建对象的方法
#### 工厂模式
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
 
#### 构造函数模式 
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
- 缺陷：每个person实例都包含一个不同的sayName Function实例

#### 原型模式
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
- 解决：方法放在原型上被所有实例共享  
- 缺陷：引用类型被所有实例共享

#### 组合使用构造函数和原型模式  
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