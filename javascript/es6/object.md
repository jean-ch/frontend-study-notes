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

#### 对象的super关键字   
- this指向方法所在的object  
- super指向object的原型  
	- super只能用在对象的方法中，不能用在对象的属性，或对象方法里面的函数中   

#### 解构赋值  
不能用于复制继承自原型对象的属性 
```
const o = Object.create({ x: 1, y: 2 });
o.z = 3;
let { x, ...newObj } = o;
let { y, z } = newObj;
x // 1
y // undefined
z // 3
```   

#### 扩展运算符 
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

#### Object.is() 
- ==会自动进行类型转换  
- Object.is和===的区别   
	- NaN !== NaN  
	- +0 === -0  

#### Object.assign(target, obj1, obj2)  
- 同名属性替换：obj2会覆盖obj1  
- 不是对象会先转成对象   
- undefined和null无法转成对象，作为首参数会报错，否则会跳过  
- 只拷贝对象的自身属性，**不拷贝继承属性和不可枚举属性**  
- 浅拷贝  
- get, set拷贝下来是undefined，因为Object.assign只拷贝属性值，不拷贝属性背后的赋值方法或取值方法  
- 把数组作为对象处理  
	```
	Object.assign([1, 2, 3], [4, 5])
	// [4, 5, 3] 4作为属性名为1的对象覆盖了前面的1
	```

#### 对象的原型链继承  
- Object.setPrototypeOf(obj, prototype)    
	设置一个对象的prototype对象
	```
	let proto = {};
	let obj = { x: 10 };
	Object.setPrototypeOf(obj, proto);
	proto.y = 20;
	proto.z = 40;
	obj.x // 10
	obj.y // 20
	obj.z // 40
	```
- Object.getProptotypeOf(obj)  
  读取一个对象的原型对象

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
