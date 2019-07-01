### Set
#### 构造
new Set(arr or interable data)    
- 数组去重```[...new Set(arr)]```   
- 字符串字符去重```[...new Set(str)].join('')```   
- Set转成数组```Array.from(set)```   
- NaN在set中视作相等   
- 指向同一个地址的对象在set中视作相等  

#### properties, functions   
- size  
- add(), has(), delete(), clear()  
- 数组的map,  filter可以间接用于set  
```
set = new Set([...set].filter(x => (x % 2) == 0));
set = new Set([...set].map(val => val * 2));
```

#### 遍历操作    
- keys()  
- values()
- [...set]    
- entries()- 键名和键值是同一个值   
- forEach- 键名和键值是同一个值　

### Map  
和Object的区别： object只能用string做key 
**对同一个object的引用，map视作同一个key**    
new Map(arr or set or interable data)  

#### 遍历操作   
```for (let [key, value] of map)```  
```for (let [key] of map)```  
```for (let [, value] of map)```  

