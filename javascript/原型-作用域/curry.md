#### curry
部分求值- 把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回一个新的函数的技术，新函数接受余下参数并返回运算结果

作用- 参数复用、提前返回和延迟执行

#### 实现
multi(2)(3)(4)   
```
function curry(fn) {
    let args = [];
    // 收集参数，直到参数个数满足函数传参个数
    return function() {
        args.concat([...arguments]);
        if (args.length >= fn.length) {
            return fn(...args);
        } else {
            return curry.apply(null, fn, args);
        }
    }
}
function multiFn(a, b, c) {
    return a * b * c;
}

var multi = curry(multiFn);
```

#### 优化- 参数个数不固定，随意传参
实现一个add方法，使计算结果能够满足如下预期：  
add(1)(2)(3) = 6   
add(1, 2, 3)(4) = 10   
add(1)(2)(3)(4)(5) = 15  

```
function add() {
    let args = [...arguments];
    function _add() {
        args = args.concat([...arguments]);
        return add.apply(null, args);
    }

    _add.toString = function() {
        return [...args].reduce((acc, cur) => acc + cur); 
    }

    return _add;
}
```