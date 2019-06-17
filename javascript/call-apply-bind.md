#### call
1. 对象的继承 
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

2. 借用方法 
```
let domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));
``` 

#### apply 
1. Math.max
```
let max = Math.max.apply(null, array);
let min = Math.min.apply(null, array);
```

2. 数组合并 
```
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

Array.prototype.push.apply(arr1, arr2);
console.log(arr1); // [1, 2, 3, 4, 5, 6]
```

#### bind  
bind 方法 与 apply 和 call 比较类似，也能改变函数体内的 this 指向。不同的是，bind 方法的返回值是函数，并且需要稍后调用，才会执行。而 apply 和 call 则是立即调用。
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