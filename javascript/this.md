#### 函数中的this
在一个函数的执行上下文中，this由该函数的调用者提供，由调用函数的方式来决定其指向   
如果调用者被某一个对象所拥有，那么在调用该函数时，内部的this指向该对象。如果调用者函数独立调用，那么该函数内部的this则指向undefined   

##### example
```
//demo01
var a = 20;
var obj = {
    a: 40
}
function fn() {
    console.log('fn this: ',this);

    function foo() {
        console.log(this.a)
    }
    foo();
}

fn.call(obj);
fn();
```  
fn.call(obj); fn的this是obj， goo的this是  

##### another example  
```
var length = 10;
function fn () {
    console.log(this.length);
}
var obj = {
    length: 5,
    method: function (fn) {
        fn();
        arguments[0]();
    }
};
obj.method(fn, 1);
```
先输出一个 10，然后输出一个 2   
- 在我们这道题中，虽然 fn 作为 method 的参数传了进来，但它的调用者并不受影响，任然是 window，所以输出了 10。
- arguments[0]();这条语句并不常见，可能大家有疑惑的点在这里。 其实，arguments 是一种特殊的对象。在函数中，我们无需指出参数名，就能访问。可以认为它是一种，隐式的传参形式。
- 当执行 arguments0; 时，其实调用了 fn()。而这时，fn 函数中 this 就指向了 arguments，这个特殊的对象。obj.method 方法接收了 2 个参数，所以 arguments 的 length，很显然就是 2 了。  