#### 方法  
- 精度：number是用64位比特表示的，相当于标准的double双精度浮点数
	- 12和12.0是一个数
	- 0.1 + 0.2 = 0.30000000000000004
- Number.isFinite(**Infinity**)  
- Number.isNaN()  
	- NaN !== NaN
	- Object.is(NaN, NaN) === true
	- 与传统全局上的 isFinite, isNaN的区别：   
		- 传统方法先调Number()    
		- Number.isFinite(), Number.isNaN()只对Number型数据有效，对于非Number型数据返回false  
- Number()  
	参数不符合数字转换规则，则返回 NaN
- Number.parseInt(value, base): base是转换格式，如 16 代表 16 进制  
	和 Number() 的区别：	
	- 忽略开始的空格字符
	- 第一个非空格自负不是数字或负号，则返回 NaN   
		```
		// 对于空字符串
		Number.parseInt('') // NaN;
		Number('') === 0
		```
	- 省略后面非数字的字符```Number.parseInt('123aaa') == 123```
	- 小数点不识别```Number.parseInt('22.5') == 22```
- Number.isInteger()  
	- 只处理Number型数据   
	- 对精度值有误判：   
		- Number.isInteger(25.0) === true // js 不区分浮点数，整数和浮点数采用同样的储存方法，因此25和25.0视作同一个值     
		- Number.isInteger(3.0000000000000002) === true //小数点后16位被丢弃了  
		- Number.isInteger(**Number.MIN_VALUE**) === false   

- Math.trunc() 去除小数部分，返回整数部分    
- Math.sign() 判断是正数负数还是零   
- Math.cbrt() 算立方根  
- 指数运算符** ```2 ** 2 === 4```
- Number.MIN_VALUE, Number.MAX_VALUE
- NaN: 不和任何值相等，包括 NaN 本身