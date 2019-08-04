#### arguments, caller， callee
- arguments.callee指向这个拥有arguments对象的函数，用于对函数名的解耦和 
- funtion_name.caller指向调用当前函数的引用，全局作用域返回null  
- arguments.callee.caller等同于function_name.caller

#### apply, call, bind
- 用于设置函数体中的this 
- 第一个参数是运行函数的作用域
	- apply() 第二个参数接受数组或arguments对象  
	- call() 第二个参数接受逐个列举的参数
- bind返回function

##### 默认参数
- 只有undefined能trigger函数参数的默认值   
- 函数的length属性返回函数没有指定默认值的参数个数   
##### rest参数
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