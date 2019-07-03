#### 与for..in的区别  
- for..in遍历keys, for..of遍历values    
- for..in遍历所有可枚举的属性(enumerable: true)，for..of只能遍历实现了iterable接口的属性
  - Object.keys() + for..of可以解决这一问题     
  ```for (var key of Object.keys(someObject))```
- for..in可遍历对象和原型链上的属性，for..of只能遍历对象本身的属性     
  - hasOwnProperty + for..in可以区分属性在对象上还是在原型上
  - for..of遍历对象原型的属性：```for(var key of Object.keys(someObject.prototype))```
#### 与forEach的区别   
forEach无法跳出循环，而for..of可以和return， break， continue结合使用   
