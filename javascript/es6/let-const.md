### let  
1. 只在块级作用域内有效    
2. 不存在变量提升  
var的变量提升： 在声明之前可以使用，value是undefined  
3. 死区
ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。  
```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```
4. 不允许重复声明  
5. 顶层声明的对象不属于全局(window)变量（区别于var和function）

### const   
const保证的是变量指向的内存地址所保存的数据不能改动而不是变量的值
- 简单类型const等于常量   
- Array和Object，改变value或者改变property都无法控制，const只能保证不把指向地址的指针改成指向另一个Array/Object   