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