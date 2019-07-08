#### static
静态方法不会被实例继承，而是直接通过类来调用  
如果静态方法包含this关键字，这个this指的是类，而不是实例

#### super
- super作为函数调用时，代表父类的构造函数
```
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```

- super作为对象时，在普通方法中，指向父类的原型对象  
在子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例
```
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```

- super作为对象时，在静态方法中，指向父类，而不是父类的原型对象  
方法内部的this指向当前的子类，而不是子类的实例
```
class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3
```

