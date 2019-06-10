### Set
#### 构造
new Set(arr or iterated data)    
- 数组去重```[...new Set(arr)]```   
- 字符串字符去重```[...new Set(str)].join('')```   
- Set转成数组```Array.from(set)```   

#### properties, functions   
- size  
- add(), has(), delete(), clear()  

#### iterate  
- keys()  
- values()
- [...set]    
- entries()- 键名和键值是同一个值   
- forEach- 键名和键值是同一个值　

### Map  
和Object的区别： object只能用string做key 
**对同一个object的引用，map视作同一个key**    
#### 构造  
- 数组```new Map([[key1, val1], [key2, val2]])```    


