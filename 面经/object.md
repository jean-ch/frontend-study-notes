##### 下面代码的输出是什么?
```
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```
答案： 456
object的key只能是string。所以把**一个非string类型的数据作为object的key，会调用这个数据的toString()方法。obj.toString === '[objrct, object]'**。所以b和c放进去是同一个key