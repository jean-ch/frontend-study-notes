####  扩展运算符的应用场景
```...arr```调用的数据结构中的iterator接口       
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
- 与解构赋值结合```const [first, ...rest] = [1, 2, 3, 4, 5];```
- 遍历Map, Set, Generator  
	```[...map.keys()];```

#### 解构赋值  
- 交换数组中的两个元素 
````[arr[i], arr[j]] = [arr[j], arr[i]]```

### API
#### 转换数组： Array.from(), 扩展运算符, Array.prototype.slice, Array.of()
- Array.from(array-like, mapFunc)
	- array-like: 
		- arguments, nodeList
		- 提供了iterator接口的数据: set, map
		- **有length属性的数据**：  
		```Array.from({ length: 3 }); // [ undefined, undefined, undefined ]```   
- 扩展运算符  
```[... arguments]``` ```[...document.querySelectAll('div')]```   
	- 只能转换有Iterator接口的数据  
- Array.prototype.slice
	- 转换没有length属性的数据
- Array.of()   
	- 把一组值转换成数组  
	```Array.of(1, 2, 3); // [1, 2, 3] ```  
	- **用于弥补Array()构造数组的不足- 因为参数个数不同导致Array()行为有差异, 参数个数只有一个代表指定 数组的长度**  
	```Array(3); //[ , , ,]```  

#### 检测数组 
- arr instanceof Array
- Array.is(arr) 
- arr.constructor === Array
- Object.prototype.toString.call(arr) === "[Object Array]"

#### concat, slice, splice
- slice return数组的浅拷贝
- splice 改变原数组，return包含所有被删除元素的array. ssplice(startIndex, count, item1, item2..)

#### push, pop, shift, unshift
- push, pop 向数组的尾端插入/删除
- unshift, shift 向数组的开头插入/删除
- push, unshift return改变后数组的length
- pop, shift return删除的元素 

#### find, includes, indexOf
- includes采用严格等号===进行比对，因此对NaN判断有误
- indexOf改进了includes，对NaN判断正确
- find(callback(element, index, arr))

#### filter, map, every, some
- 第一个参数是callback函数，callback都接受三个参数 element, index, 原array
- 第二个参数指定callback里的this

#### reduce
arr.reduce(callback(accumulator, currentValue, index), initialValue)
- accumulator是滚动运算的前次运算的值， 或initialValue
- reduce可以带initialValue

##### reduce实现map
```
function reduceMap(func, that) {
	if (typeof func != 'function') {
		throw new TypeError(func + ' is not a function');
	}

	if (!Array.isArray(this)) {
		throw new TypeError(this + ' is not an array');
	}

	if (this.length == 0) {
		return [];
	}

	return this.reduce((acc, cur, index, arr) => {
		return acc.concat([func.call(that, cur, index, arr)]);
	}, []); 
}
```

##### reduce实现filter
```
Array.prototype.filter = function(func, that) {
	return this.reduce((acc, cur, index, arr) => {
		return func.call(that, cur, index, arr) ? acc.concat([cur]) : acc；
	}, []);
}
```

##### map实现reduce
```
Array.prototype.reduce = function(func, init) {
	let list = [init];
	let acc = init;
	this.map((cur, index, arr) => {
		acc = func(acc, cur, index, arr);
		list.push(acc);
	});
	return list[list.length - 1];
}
```

#### sort, reverse
- sort(function compare(a, b) {})    
- reverse()

#### iterator
- arr.keys()
- arr.values()
- arr.forEach(callback(element, index, arr))

#### fill
- fill(value, startIndex, endIndex)

#### flat
- flat(depth) 

#### Interview Questions
- 打乱数组
```
const arr = [0, 1, 2, 3, 4];
for (let i = 1; i < arr.length; i++) {
    const random = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[random]] = [arr[random], arr[i]];
}
```
