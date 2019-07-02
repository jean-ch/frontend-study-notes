#### 与for..in的区别  
- for..in遍历keys, for..of遍历values    
- 普通没有实现iterator接口的object用for..in依然可以遍历keys，但用for..of无法遍历values.  
  只要是可枚举的属性(enumerable: true) for..in都可以遍历到。区分interable对象和可枚举属性
  - 解决办法： 用Object.keys()生成一个keys的数组  
    ```
    for (var key of Object.keys(someObject)) {
      console.log(key + ': ' + someObject[key]);
    }
    ```   
  - 如此一来，for..of只能遍历对象本身的属性，而for..in还能遍历对象原型上的属性  
    ```for(var key of Object.keys(someObject.prototype))```

#### 与forEach的区别   
forEach无法跳出循环，而for..of可以和return， break， continue结合使用   
