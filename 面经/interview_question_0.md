##### 求输出结果 
```
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
}

console.log(new Date, i);
```
输出5,5,5,5,5,5  

##### 用箭头表示其前后的两次输出之间有 1 秒的时间间隔，而逗号表示其前后的两次输出之间的时间间隔可以忽略，代码实际运行的结果该如何描述？ 
5 -> 5,5,5,5,5，即第 1 个 5 直接输出，1 秒之后，输出 5 个 5
循环执行过程中，几乎同时设置了 5 个定时器，一般情况下，这些定时器都会在 1 秒之后触发，而循环完的输出是立即执行的  

##### 如果期望代码的输出变成：5 -> 0,1,2,3,4，该怎么改造代码 
###### 利用闭包
```
for (var i = 0; i < 5; i++) {
    (function(j) {  // j = i
        setTimeout(function() {
            console.log(new Date, j);
        }, 1000);
    })(i);
}

console.log(new Date, i);
```   

###### 利用按值传递 
```
var output = function (i) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
};

for (var i = 0; i < 5; i++) {
    output(i);  // 这里传过去的 i 值被复制了
}

console.log(new Date, i);
```  

##### 如果期望代码的输出变成 0 -> 1 -> 2 -> 3 -> 4 -> 5，并且要求原有的代码块中的循环和两处 console.log 不变，该怎么改造代码
###### promise 
```
const tasks = [];
for (var i = 0; i < 5; i++) {   // 这里 i 的声明不能改成 let，如果要改该怎么做？
    ((j) => {
        tasks.push(new Promise((resolve) => {
            setTimeout(() => {
                console.log(new Date, j);
                resolve();  // 这里一定要 resolve，否则代码不会按预期 work
            }, 1000 * j);   // 定时器的超时时间逐步增加
        }));
    })(i);
}

Promise.all(tasks).then(() => {
    setTimeout(() => {
        console.log(new Date, i);
    }, 1000);   // 注意这里只需要把超时设置为 1 秒
});
``` 
```
const tasks = []; // 这里存放异步操作的 Promise
const output = (i) => new Promise((resolve) => {
    setTimeout(() => {
        console.log(new Date, i);
        resolve();
    }, 1000 * i);
});

// 生成全部的异步操作
for (var i = 0; i < 5; i++) {
    tasks.push(output(i));
}

// 异步操作完成之后，输出最后的 i
Promise.all(tasks).then(() => {
    setTimeout(() => {
        console.log(new Date, i);
    }, 1000);
});
```

###### async
```
// 模拟其他语言中的 sleep，实际上可以是任何异步操作
const sleep = (timeountMS) => new Promise((resolve) => {
    setTimeout(resolve, timeountMS);
});

(async () => {  // 声明即执行的 async 函数表达式
    for (var i = 0; i < 5; i++) {
        await sleep(1000);
        console.log(new Date, i);
    }

    await sleep(1000);
    console.log(new Date, i);
})();
```

