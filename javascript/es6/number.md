#### 方法  
- Number.isFinite(Infinity)  
- Number.isNaN()  
- Number.parseInt()  
- Number.isInteger()  
对精度值有误判：  
	- Number.isInteger(3.0000000000000002) === true
	- Number.isInteger(Number.MIN_VALUE) === false

- Math.trunc() 去除小数部分，返回整数部分    
- Math.sign() 判断是正数负数还是零   
- Math.cbrt() 算立方根  
- 指数运算符** ```2 ** 2 === 4```