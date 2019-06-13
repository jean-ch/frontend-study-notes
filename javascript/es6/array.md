####  扩展运算符
```...arr```  
- 应用场景   
	- 数组平铺成函数参数   
	- 利用Math.max求数组最大值```Math.max(...arr)```  
	- 合并数组(浅拷贝) 
		```
		arr1.concat(arr2, arr3) //es5
		arr1.push(...arr2)； //es6
		[...arr1, ...arr2, ...arr3]; // es6
		```  
	- 克隆数组  
		``` 
		const arr1 = [1, 2];
		const arr2 = [...arr1]; //写法1
		const [...arr2] = arr1; //写法2
		```   
	- 字符串转成字符数组 ```[...'hello']```    
	- 把实现了Iterator接口的对象转化成数组   
		```[...document.querySelectorAll('div')]; //querySelector得到的是实现了Iterator的类数组```   
	- 遍历Map, Set, Generator  
		```[...map.keys()];```

#### Array.from()  
- 第二个参数提供map功能  
- 把Array-like数据转成数组： dom操作和querySelectAll操作返回的nodelist，函数的argument   
- 把实现了Iterator接口的对象转成数组： Map, Set  
- 转换类似数组的对象，即**只要有length属性，就可以被Array.from转换**   
	```Array.from({ length: 3 }); // [ undefined, undefined, undefined ]```   
	- 扩展运算符也可以将某些数据转成数组   
		```[... arguments]``` ```[...document.querySelectAll('div')]```   
	- 但扩展运算符只能转换有Iterator接口的数据  
- **没有length属性的数据想要转换成数组，替代方法是Array.prototype.slice**```[].slice.call(obj)```   

#### Array.of()   
- 把一组值转换成数组
	```Array.of(1, 2, 3); // [1, 2, 3] ```  
- **用于弥补Array()构造数组的不足- 因为参数个数不同导致Array()行为有差异, 参数个数只有一个代表指定 数组的长度**
	```Array(3); //[ , , ,]```   

#### includes()  
**和indexOf的区别： indexOf采用严格等号=== ，因此会导致对NaN的误判**    

#### Iterator接口  
- keys()- [index]   
- values()- [value]  
- entries()- [[index, value]..]    

#### 其他  
- find(), findIndex()     
- fill()   
- flat()  
