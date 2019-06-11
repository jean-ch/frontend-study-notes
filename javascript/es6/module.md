#### 写法 
```
// 大括号一定要有
export { firstName, lastName, year };
import { firstName, lastName, year } from './profile.js';

import * as circle from './circle'; //整体加载

export default function(){} // default只能有一个 
import customName from './export-default'; // import的时候名字自己取 
```   

#### modeule一定是严格模式的 
所以顶层没有this，顶层的this指向undefined   

#### ES6 module和CommonJS的区别  
- CommonJS是值拷贝，Module是值的引用- 内部模块的改变动态影像这个值  
- CommonJs是运行时加载，ES6 module是静态加载  
因为CommonJS加载的是对象，脚本运行完才会生成
Module加载的是接口，接口是静态定义，静态解析时就可以完成  

#### 浏览器加载  
在script标签中加入type="module"表示异步加载，不会阻塞浏览器  
```<script type="module" src="./foo.js"></script>```  