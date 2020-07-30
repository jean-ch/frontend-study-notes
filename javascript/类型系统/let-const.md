### let  
```let 和 const 拥有块级作用域```
0. 必须要在严格模式下使用  
1. 只在块级作用域内有效    
2. 不存在变量提升  
var的变量提升： 在声明之前可以使用，value是undefined  
3. 暂时性死区
    ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。  
    ```
    var tmp = 123;
    if (true) {
      tmp = 'abc'; // ReferenceError   
      let tmp;
    }
    ```
    这说明let其实还是在块级作用域内提升了，不然应该可以访问到全局的tmp  
    但是**提升的只是创建，没有初始化**，因此不能在声明之前使用   
4. 不允许重复声明  
5. 顶层声明的对象不属于全局(window)变量（区别于var和function）

##### example
```
var arr = [];
for(var i = 0; i < 2; i++){
    arr[i] = function(){
        console.log(i);
    };
};
arr[1]();
```
输出2.因为var变量提升了，当执行arr[1]()时， i取自父元素的i，已经是2了  
传统解决方法是用闭包   
```
var arr = [];
for(var i = 0; i < 2; i++){
    arr[i] = (function(i){
        return function(){
            console.log(i);
        };
    }(i));
};
arr[1]();
```
es6用let就可以解决 
```
'use strict';
var arr = [];
for(let i = 0; i < 2; i++){
    arr[i] = function(){
        console.log(i);
    };
};
arr[1]();
```

#### let在循环中每次循环都赋予新的值
```
for(let i = (setTimeout(()=>console.log(i), 2333) , 0); i < 2; i++) {
  
}
```
打印0而不是2.因为let传给setTimeout回调函数的值是第一次循环的0.如果是var就是打印2  

### const   
const保证的是变量指向的内存地址所保存的数据不能改动而不是变量的值
- 简单类型const等于常量   
- Array和Object，改变value或者改变property都无法控制，const只能保证不把指向地址的指针改成指向另一个Array/Object   