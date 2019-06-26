##### 下面代码输出什么？
```
var foo = function bar(){ return 12; };
typeof bar();  
```
输出是抛出异常，bar is not defined。
如果想让代码正常运行，需要这样修改代码：
```
var bar = function(){ return 12; };
typeof bar();  
```
或者是
```
function bar(){ return 12; };
typeof bar();  
```
明确说明这个下问题
```
var foo = function bar(){ 
    // foo is visible here 
    // bar is visible here
    console.log(typeof bar()); // Work here :)
};
// foo is visible here
// bar is undefined here
```

##### 下面代码输出什么？
```
var salary = "1000$";
(function () {
    console.log("Original salary was " + salary);

    var salary = "5000$";

    console.log("My New Salary " + salary);
})();
```
输出是
Original salary was undefined
My New Salary 5000$
这题考察的是**变量提升**。等价于以下代码
```
var salary = "1000$";
 (function () {
     var salary ;
     console.log("Original salary was " + salary);

     salary = "5000$";

     console.log("My New Salary " + salary);
 })();
 ```