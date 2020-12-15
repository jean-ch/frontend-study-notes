#### 扩展运算符的应用场景

`...arr`调用的数据结构中的 iterator 接口

- 数组平铺成函数参数
- 利用 Math.max 求数组最大值`Math.max(...arr)`
- 合并数组(浅拷贝)
  `arr1.concat(arr2, arr3) //es5 arr1.push(...arr2)； //es6 [...arr1, ...arr2, ...arr3]; // es6`
- 克隆数组  
   `const arr1 = [1, 2]; const arr2 = [...arr1]; //写法1 const [...arr2] = arr1; //写法2`
- 字符串转成字符数组 `[...'hello']`
- 把实现了 Iterator 接口的对象转化成数组  
   `[...document.querySelectorAll('div')]; //querySelector得到的是实现了Iterator的类数组`
- 与解构赋值结合`const [first, ...rest] = [1, 2, 3, 4, 5];`
- 遍历 Map, Set, Generator  
   `[...map.keys()];`

#### 解构赋值

- 交换数组中的两个元素
  ``[arr[i], arr[j]] = [arr[j], arr[i]]```

### API

#### 构造数组

```
var arr = ['a', 'b'];
var arr = Array('a', 'b');
var arr = new Array('a', 'b');
```

#### length

length 不是只读的

```
var arr = [1, 2, 3];
arr.length = 2;
alert(arr[2]) // undefined
```

#### 转换数组： Array.from(), 扩展运算符, Array.prototype.slice, Array.of()

- Array.from(array-like, mapFunc) - array-like: - arguments, nodeList - 提供了 iterator 接口的数据: set, map - **有 length 属性的数据**：  
   `Array.from({ length: 3 }); // [ undefined, undefined, undefined ]`
- 扩展运算符  
  `[... arguments]` `[...document.querySelectAll('div')]`  
   - 只能转换有 Iterator 接口的数据
- Array.prototype.slice - 转换没有 length 属性的数据
- Array.of()  
   - 把一组值转换成数组  
   `Array.of(1, 2, 3); // [1, 2, 3]`  
   - **用于弥补 Array()构造数组的不足- 因为参数个数不同导致 Array()行为有差异, 参数个数只有一个代表指定 数组的长度**  
   `Array(3); //[ , , ,]`
- valueOf()  
   `[1, 2].valueOf() // [1, 2]`
- toString()  
   `[1, 2].toString() // "1, 2"`

#### 检测数组

- arr instanceof Array
- Array.is(arr)
- arr.constructor === Array
- Object.prototype.toString.call(arr) === "[Object Array]"

#### concat, slice, splice

- slice return 数组的浅拷贝
- splice 改变原数组，return 包含所有被删除元素的 array. splice(startIndex, count, item1, item2..)

#### push, pop, shift, unshift

- push, pop 向数组的尾端插入/删除
- unshift, shift 向数组的开头插入/删除
- push, unshift return 改变后数组的 length
- pop, shift return 删除的元素

#### find, includes, indexOf

- includes 采用严格等号===进行比对，因此对 NaN 判断有误. includes(val, fromIndex)， 参数不是 callback func
- indexOf 改进了 includes，对 NaN 判断正确, indexOf(val, fromIndex)
- find(callback(element, index, arr))

#### filter, map, every, some

- 第一个参数是 callback 函数，callback 都接受三个参数 element, index, 原 array
- 第二个参数指定 callback 里的 this

#### reduce

arr.reduce(callback(accumulator, currentValue, index), initialValue)

- accumulator 是滚动运算的前次运算的值， 或 initialValue
- reduce 可以带 initialValue, return value to find / undefined

##### reduce 实现 map

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

##### reduce 实现 filter

```
Array.prototype.filter = function(func, that) {
	return this.reduce((acc, cur, index, arr) => {
		return func.call(that, cur, index, arr) ? acc.concat([cur]) : acc；
	}, []);
}
```

##### map 实现 reduce

```
Array.prototype.reduce = function(func, init) {
	if (this.length === 0) {
		throw new Error();
	}

	let acc = init ? init : this[0];
	let array = init ? this : this.slice(1);
	let list = this.map((cur, index, arr) => {
		acc = func(acc, cur, index, arr);
		return acc;
	});

	return list;
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
