#### this的绑定优先级
- 指向new出来的实例对象
    ```
    function F() {
            this.name = 1
    }
    var f = new F()
    ```
- call、apply、bind 指向传入函数的第一个参数
- 作为对象的方法调用时指向对象本身
    ```
    var obj = {
        value: 5,
        printThis: function () {
            console.log(this.value);
        }
    };
    // 隐式绑定this到上下文对象obj上
    obj.printThis(); // 5
    ```
- 按值传递 指向全局
    ```
    var value = 10;
    var obj = {
        value: 5,
        printThis: function () {
            console.log(this.value);
        }
    };
    f = obj.printThis
    // 隐式丢失：fn调用的时候上下文没有指明，因此不是obj而是windows
    f(); // 10
    ```
    ```
    function foo() {
        console.log( this.a );
    }
    function doFoo(fn) {
        fn(); //在此处调用，参数传递是隐式赋值，丢失this绑定
    }
    var obj = {
        a: 2,
        foo: foo
    };
    var a = "opps, global";
    doFoo( obj.foo );   //看似是隐式绑定，输出opps, global
    ```
- 匿名函数的this是windows

#### 面试题
##### 方法调用this指向调用方法的对象，函数调用this指向window
```
var a = 20;
var obj = {
    a: 40
}
function fn() {
    console.log('fn this: ',this);

    function foo() {
        console.log(this.a)
    }
    foo(); //这里foo是函数调用，无论fn的this是什么，foo的this始终是window
}
fn.call(obj); 
fn(); 
```  
20, 20
##### arguments[0]\(\)是函数调用，this指向arguments
```
var length = 10
function fn() {
    console.log(this.length)
}
var obj = {
    length: 5,
    method: function (fn) {
        fn();    // 方法调用，this指向windows
        arguments[0]();  // 函数调用，this指向arguments
    }
}
obj.method(fn, 10, 5, 2, 6); 
```
10, 5

##### 数组的方法调用，this指向数组

```
var length = 10;
var age = 18;
function fn() {
    console.log(this.length); 
}
var arr = [fn, "222"];

fn();
arr[0](); // 数组也是对象，fn是数组对象arr的方法，方法名为index 0
```
10, 2

##### 匿名函数调用方式this指向的window
```
var x = 3;
var y = 4;
var obj = {
     x: 1,
     y: 6,
     getX: function () {
         var x = 5;
         return function () {
             return this.x; 
         }(); // 匿名函数调用方式this指向的window
     },
     getY: function () {
         var y = 7;
         return this.y; 
     }
 }
 console.log(obj.getX())
 console.log(obj.getY())
```
3, 6

##### 多层对象，方法指向最紧邻的对象
```
var a = 'javascript';
var obj = {
    a : 'php',
    prop : {
        getA : function() {
            return this.a;
        }
    }
}

console.log(obj.prop.getA()); // this指向的obj.prop.这个作用域里没有变量a, 所以为undefined
var text = obj.prop.getA;
console.log(text());
```
undefined, javascript

#### 区别this和作用域链
##### this只能指向当前活动对象，不能往外部函数找
```
var count = 1;
var obj = {
	func: function() {
		console.log(this.count);
	}
};
obj.func(); // undefined
```
##### 不带this则顺着作用域链往上找
```
var count = 1;
var obj = {
	func: function() {
		console.log(count);
	}
};
obj.func(); // 1
```
##### 对象函数里面，this指向对象，不带this作用域链指向全局对象
```
var count = 1;
var obj = {
    count: 2,
	func: function() {
		console.log(count);
	}
};

obj.func(); // 1
```
```
var count = 1;
var obj = {
    count: 2,
	func: function() {
		console.log(this.count);
	}
};

obj.func(); // 2
```
##### 闭包函数里面，this指向window（闭包是匿名函数），作用域链指向当前活动对象->外部函数活动对象
```
var count = 1;
function f1() {
	var count = 2;
	return function() {
		console.log(count);
	}
}
f1()(); //2
```
```
var count = 1;
function f1() {
	var count = 2;
	return function() {
		console.log(this.count);
	}
}
f1()(); //1
```