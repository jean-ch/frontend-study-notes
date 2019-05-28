#### undefined vs null  
- undefined: 未定义的空值 
- null： 已定义但值为空 

#### undefined vs void 0
undefined == void 0
但是因为js设计初的失误，undefined不是一个关键字而是一个变量，所以会出现可以对undefined重新赋值的情况 
```
console.log (undefined);
var undefined = 1;
console.log (undefined); // 1
```