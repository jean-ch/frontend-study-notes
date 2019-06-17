##### 下面代码输出什么？  
```
var output = (function(x){
    delete x;
    return x;
})(0);
  
console.log(output);
```  
输出是 0。 delete 操作符是将object的属性删去的操作。但是这里的 x 是并不是对象的属性, delete 操作符并不能作用。  
**delete operator removes a property from an object**  

##### 下面代码输出什么？
```
var Employee = {
    company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```  
输出是 xyz，这里的 emp1 通过 prototype 继承了 Employee的 company。emp1自己并没有company属性。所以delete操作符的作用是无效的。

##### 下面代码输出什么？
```
var trees = ["xyz","xxxx","test","ryan","apple"];
delete trees[3];
  
console.log(trees.length);
``` 
输出是5。因为delete操作符并不是影响数组的长度。删去的index为3的元素现在是个empty item  

