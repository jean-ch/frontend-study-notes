- 只有undefined能trigger函数参数的默认值   
- 函数的length属性返回函数没有指定默认值的参数个数   
- rest参数 ```function add(...values){}```  
	- rest是个数组， 相对于arguments只是个类数组，要用Array.prototype.slice.call(arguments)转换成数组   
	- rest参数放在所有参数的最后  
- name属性返回函数名
	- 匿名函数赋值给变量，name属性返回变量名
	``` 
	var f = function(){}; 
	f.name === "" //es5
	f.name === "f" //es6
	```
	- Function构造的函数返回anonymous```(new Function).name === "anonymous"```   
	- bind返回的函数，name属性会加上bound前缀  
	```
	function foo(){};
	foo.bind({}).name === "bound foo";
	(function(){}).bind({}).name === "bound";
	```  

#### 箭头函数   
- 只有一个参数，参数不用打括号。但是没有参数或者有多个参数也就要打括号  
- 只有一行语句可以不用写return。但是多行语句就要用大括号括起来并且要return    
- 可以结合rest参数传多个参数  
- 直接返回一个对象需要在对象外面就加上括号  
```
let getItem = id => {id: id, name: name}; //Error
let getItem = id => ({id: id, name: name});
```
- 箭头函数**没有自己的this, arguments, super, constructor**等
	- 箭头函数的this是外层代码块的this    
	- 无法用call(), apply(), bind()这些方法来改变this  

##### 箭头函数的不适用场景  
需要动态上下文场景时不使用箭头函数  
1. 对象的方法  
因为对象不构成作用域，所以对象中的箭头函数this指向的不是对象而是全局   
```
const obj = {
    x: 1,
    print: () => {
        console.log(this === window); // => true
        console.log(this.x); // undefined
    }

    print () {
	    console.log(this === test); // => true
	    console.log(this.x); // 1
	}
};
```

2. 原型方法 
```
function Cat (name) {
    this.name = name;
}

Cat.prototype.sayCatName = () => {
    console.log(this === window); // => true
    return this.name; // undefined
};

Cat.prototype.sayCatName = function () {
    console.log(this === cat); // => true
    return this.name; // Cat.name
};
```  
3. 事件的回调  
```
const btn = document.getElementById('myButton');
btn.addEventListener('click', () => {
  console.log(this === window); // => true
  this.innerHTML = 'Clicked button';
});
```
需要用发生时间的上下文（即上例的btn）来处理函数   
**为事件回调函数绑定this**     
- bind(this) 
```
<Button onClick={this.handleClickButton.bind(this)}>
handleClickButton () {}
```   
- ```<Button onClick={ event => this.handleClickButton(event) }>```  
4. 构造函数    
构造函数没有constructor方法，不能用new关键字调用    
```
const Message = (text) => {
    this.text = text;
};

var helloMessage = new Message('Hello World!');
// Uncaught TypeError: Message is not a constructor
```