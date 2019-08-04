#### 单层数组深拷贝
1. ```copyArr = arr.slice()``` 
2. ```copyArr = arr.concat()```  

#### 单层对象深拷贝 
1. ```copyObj = Object.assign({}, obj)```   
2. ```copyObj = {...obj}```   

#### 多层深拷贝  
1. JSON  
```JSON.parse(JSON.stringify(obj))```
2. recursive
```
function isObj(obj) {
	return obj !== null && (typeof obj === 'object' || typeof obj === 'function');
}

function deepCopy(obj) {
	let copyObj = Array.isArray(obj) ? [] : {};
	for (let key in obj) {
		copyObj[key] = isObj(obj[key]) ? deepCopy(obj[key]) : obj[key];
	}

	return copyObj;
}
```

#### 以上两种方法的问题  
1. 特殊类型：function无法拷贝，date拷贝成字符串，reg拷贝成空对象  
2. obj中有环

#### 解决obj中有环  
```
function isObj(obj) {
	return obj !== null && (typeof obj === 'object' && typeof obj === 'function');
}

function deepCopy(obj, hash = new WeakMap()) {
	if (hash.has(obj)) {
		return hash.get(obj);
	}

	let copyObj = Array.isArray(obj) ? [] : {};
	for (let key in obj) {
		copyObj[key] = isObj(obj[key]) ? deepCopy(obj[key], hash) : obj[key]; 
	}

	hash.set(obj, copyObj);
	return copyObj;
}
```


#### 解决特殊种类对象  
```
function deepCopy(obj, hash = new WeakMap()) {
	let copyObj;
	let Constructor = obj.constructor;
	switch(Constructor) {
		case RegExp:
			copyObj = new Constructor(obj);
			break;
		case Date:
			copyObj = new Constructor(obj.getTime());
			break;
		default:
			if (hash.has(obj)) {
				return hash.get(obj);
			}
			copyObj = new Constructor();
			hash.set(obj, copyObj);
	}

	for (let key in obj) {
		copyObj[key] = isObj(obj[key]) ? deepCopy(obj[key], hash) : obj[key];
	}

	return copyObj;
}
``` 
还是没有解决function的拷贝问题，不知道怎么解决  
