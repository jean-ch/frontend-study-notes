#### call
##### 模拟实现 
```
Function.prototype.call = function(context) {
    context = context ? Object(context) : window;
    context.fn = this;
    let args = [...arguments].slice(1);
    r = context.fn(...args);
    delete context.fn;
    return r;
}
```
##### 应用
- 对象的继承 
```
function superClass () {
    this.a = 1;
    this.print = function () {
        console.log(this.a);
    }
}

function subClass () {
    superClass.call(this);
    this.print();
}

subClass();
// 1
```
- 验证是否是数组    
```String.prototype.toString.call(obj) === '[Object Array]'```
- 类/伪数组使用Array API    
```Array.prototype.slice.call(nodeList) // 将伪数组数组转化成标准数组```

#### apply 
##### 模拟实现 
```
Function.prototype.apply = function(context, args) {
    context = context ? Object(context) : window;
    context.fn = this;
    if (!args) {
        return context.fn();
    }

    let r = context.fn(...args);
    delete context.fn;
    return r;
}
```
##### 应用
- 获取数组中的最大值最小值  
```max = Math.max.apply(null, array);```

- 把一个数组的元素push进另一个数组    
```Array.prototype.push(arr1, arr2)```

#### bind  
bind方法的返回值是函数，需要稍后调用才会执行。而 apply 和 call 则是立即调用
```
function add (a, b) {
    return a + b;
}

function sub (a, b) {
    return a - b;
}

add.bind(sub, 5, 3); // 这时，并不会返回 8
add.bind(sub, 5, 3)(); // 调用后，返回 8
```
##### bind的实现
- step1. bind能够返回方法, 参数可以从bind里传也可以call 返回的方法时传
```
Function.prototype.bind(context) {
    let that = this;
    let bindArgs = Array.prototype.slice.call(arguments, 1);
    return function() {
        let args = bindArgs.concat(Array.prototype.slice.call(arguments, 1));
        return that.apply(context, args); 
    }
}
```

- step2. 还可以把返回的函数用new当一个类来调用
```
Function.prototype.bind(context) {
    let that = this;
    let bindArgs = Array.prototype.slice.call(arguments, 1);
    function fn(){} // Object.create的实现原理
    function fBind() {
        let args = bindArgs.concat(Array.prototype.slice.call(arguments, 1));
        return that.apply(context, args);
    }

    Fn.prototype = this.prototype;
    fBind.prototype = new Fn();
    return fBind;
}
```

[Reference](https://juejin.im/post/5c7fe2bcf265da2d9c38991f)