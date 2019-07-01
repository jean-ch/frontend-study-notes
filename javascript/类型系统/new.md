#### new操作符里面发生了什么
```var obj = new O();```
- 创建一个空对象obj
- 把函数的原型赋给obj： ```obj.__ptoto__ = O.prototype;``` or ```Object.setPrototypeOf(obj, O);```
- O.call(obj), this指向obj，因此把O中的properties绑定到obj上
- 构造函数不要有返回值
  - 如果返回简单数据，返回值无效，obj是第一步创建的那个对象
  ```
    function Test(name) {
    this.name = name
    return 1
  }
  const t = new Test('yck')
  console.log(t.name) // 'yck' // Test的返回值无效了
  ```
  - 如果O的return是Object，就把这个Object返回给obj，这样会导致构造函数错误
  ```
  function Test(name) {
    this.name = name
    console.log(this) // Test { name: 'yck' }
    return { age: 26 }
  }

  const t = new Test('yck')
  console.log(t) // { age: 26 } // new返回了Test的return
  console.log(t.name) // 'undefined' // new的前三步失效了
  ```