##### 下面代码的输出是什么？
```
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```
答案： undefined 和 ReferenceError
- var有变量提升，**在创建阶段会给name分配值undefined**，因此输出undefined。注意变量提升是声明提升赋值不提升哦
- let不会变量提升，因此ReferenceError  
    
变量的赋值可以分为三个阶段：    
- 创建变量，在内存中开辟空间
- 初始化变量，将变量初始化为undefined
- 真正赋值    

关于let、var和function：
- let 的「创建」过程被提升了，但是初始化没有提升。
- var 的「创建」和「初始化」都被提升了。
- function 的「创建」「初始化」和「赋值」都被提升了。