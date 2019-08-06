#### call
##### 用法
```
var person = {
    value : 1
}
function bar() {
    console.log(this.value)
}
bar.call(person) // 1
```
- 如果不将person绑定到bar的this上，this指向windows，返回undefined
- 在person对象上调用bar相当于在person上添加了一个属性指向bar，调用之后再删除
```
var person = {    
    value: 1,
    bar: function() {
        console.log(this.value)
    }
}
person.bar() // 1
delete person.bar;
```
##### 模拟实现 
- this是调用的函数，用typeof 判断this是否是一个函数
- context是调用函数的执行环境, 没有传context的话context理应指向window
- 相当于在调用函数的context上增加一个函数方法（也就是this），取得调用结果后再删除
```
Function.prototype.call(context) {
    if (typeof this !== 'function') {
        return;
    }

    context = context ? Object(context) : window;
    let args = [...arguments].slice(1);
    context.fn = this; // context- person, this- bar
    let res = context.fn(args); 
    delete context.fn;
    return res;
}
```

#### apply 
##### 模拟实现 
- 在call的基础上额外判断参数是否存在
```
Function.prototype.apply(context, args) {
    if (typeof this !== 'function') {
        return;
    }

    context = context ? Object(context) : window;
    context.fn = this;
    let res;
    if (!args) {
        res = context.fun(); 
    } else {
        res = context.fn(...args);
    }

    delete context.fn;
    return res;
}
```
#### call, apply的应用
- 验证是否是数组    
```String.prototype.toString.call(obj) === '[Object Array]'```

- 类/伪数组使用Array API    
```Array.prototype.slice.call(nodeList) // 将伪数组数组转化成标准数组```

- 获取数组中的最大值最小值  
```max = Math.max.apply(null, array);```

- 把一个数组的元素push进另一个数组    
```Array.prototype.push.apply(arr1, arr2)```

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
- bind的参数可以从两个地方传，一个是bind的时候传，一个是调用bind返回的方法时传，两处参数要合并
- 可以在bind返回的函数上new操作
    - 判断是否是new操作
        - new操作时this指向fBind的实例，self指向fBind。用this instanceof self可以判断是否采用了new操作
    - new操作this绑给创建的实例对象
        - 如果采用new操作，apply传入的this指向创建的实例对象，也就是this
        - 如果没有采用new操作，apply传入bind指定的context
    - new操作连接函数fBind和函数实例的prototype
        - 用空函数fn做中间量，防止直接赋值fBind.prototype = this.prototype导致fBind.prototype改变this.prototype也跟着改变
        - self.prototype赋值给fn.prototype
        - 然后用fBind.prototype = new Fn()实现new操作的继承
```
Function.prototype.bind(context) {
    let self = this;
    if (typeof self !== 'function') {
        return;
    }

    context = context ? Object(context) : window;
    let bindArgs = [...arguments].slice(1);
    function fBind(){
        let args = [...arguments].concat(bindArgs);
        return self.apply((this instanceof self ? this : context), args);
    }

    function Fn() {}; // Object.create()的原理。空函数做中间人连接this.prototype,这样fBind.prototype改变不会影响this.prototype(相比较直接赋值fBind.prototype = this.prototype)
    fn.prototype = self.prototype;
    fBind.prototype  = new Fn();
    return fBind;
}
```