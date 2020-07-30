typeof一共有八种返回值
- number
- boolean
- string
- undefined
- object（对象， null）
- function
- symbol

#### undefined   
```
typeof undefined === 'undefined';
typeof undeclaredVar === 'undefined';
```   

#### boolean  
```
typeof true === 'boolean';  
typeof Boolean(1) === 'boolean'; 
```  

#### number 
```
typeof 6 === 'number'; 
typeof Number('6') === 'number'; 
typeof NaN === 'number';  
```  
#### string 
```
typeof '' === 'string'; 
typeof String(1) === 'string'; 
typeof (typeof 1) === 'string'; 
```  

#### null - object
```typeof null === 'object';``` 

#### object 
```
typeof {a: 1} === 'object';
typeof [1, 2, 3] === 'object'; // 用Array.isArray, instanceOf, Object.prototype.toString.call来判断是Array 
typeof new Date() === 'object'; 
typeof /regex/ === 'object'; 
``` 

#### function 
```
typeof function() {} === 'function'; 
typeof class C {} === 'function';  
``` 

#### new - object or function
```
typeof new String('String') === 'object'; 
typeof new Number(1) === 'object';
typeof new Function() === 'function';  
``` 

#### parentheses 
```
typeof 99 + 'Wisen' === 'number Wisen';
typeof (99 + 'Wisen') === 'string'; 
``` 

#### let const Error 
```
typeof var1; //ReferenceError
typeof var2; //ReferenceError
let var1;
const var2 = 'hello'; 
```

