#### 方法  
- Number.isFinite(**Infinity**)  
- Number.isNaN()  
	- NaN !== NaN
	- Object.is(NaN, NaN) === true
	- 与传统全局上的isFinite, isNaN的区别：   
		- 传统方法先调Number()    
		- Number.isFinite(), Number.isNaN()只对Number型数据有效，对于非Number型数据返回false   
- Number.parseInt()  
	和Number的区别：	
	- 省略开始的空字
	- 只识别数字，符号，其他return NaN```Number.parseInt('') == NaN```
	- 省略后面非数字的字符```Number.parseInt('123aaa') == 123```
	- 小数点不识别```Number.parseInt('22.5') == 22```
- Number.isInteger()  
	- 只处理Number型数据   
	- 对精度值有误判：   
		- Number.isInteger(25.0) === true // 整数和浮点数采用同样的储存方法，因此25和25.0视作同一个值     
		- Number.isInteger(3.0000000000000002) === true //小数点后16位被丢弃了  
		- Number.isInteger(**Number.MIN_VALUE**) === false   

- Math.trunc() 去除小数部分，返回整数部分    
- Math.sign() 判断是正数负数还是零   
- Math.cbrt() 算立方根  
- 指数运算符** ```2 ** 2 === 4```