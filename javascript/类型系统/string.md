#### String 和 toString 的差别
String() 可以用来转换 null 和 undefined  
toString(base) 可以基于 base 转数字的进制字符串

#### 遍历 
```for (let c of s)```  

#### 模板字符串(用反引号表示)  
- 多行字符串  
```
`In Javascript this is 
not legal.`
```
- 含变量的字符串 
```
`Hello ${name}` 
`${obj.x + obj.y}` //大括号内可以放任意表达式  
```  

- 标签模板
```
let a = 5;
let b = 10;

tag`Hello ${ a + b } world ${ a * b }`;
// 等同于
tag(['Hello ', ' world ', ''], 15, 50);
```

#### 方法 
- indexOf(), includes()
- split()
- search(regex), replace(refex, str)
- startsWith(), endsWith() 
- repeat(n)   
- trim(), trimStart(), trimEnd()  
- charCodeAt() // A -> 65
- fromCharCode() // 65 -> A