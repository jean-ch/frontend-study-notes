#### 与for..in的区别  
for..in遍历keys   
for..of遍历values  
普通没有实现iterator接口的object用for..in依然可以遍历keys，但用for..of无法遍历values.
解决办： 用Object.keys()生成一个keys的数组  
```
for (var key of Object.keys(someObject)) {
  console.log(key + ': ' + someObject[key]);
}
```   

#### 与forEach的区别   
forEach无法跳出循环，而for..of可以和return， break， continue结合使用   
