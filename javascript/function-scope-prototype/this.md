#### this的指向
- 指向new出来的对象
```
function F() {
        this.name = 1
}
var f = new F()
```
- call、apply、bind 指向传入函数的第一个参数
- 对象的方法 对象内部的方法指向对象本身
```
var obj = {
    value: 5,
    printThis: function () {
        console.log(this.value);
    }
};
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
f(); // 10
```