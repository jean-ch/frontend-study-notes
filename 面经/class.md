##### 下面代码的输出是什么?
```
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
freddie.colorChange("orange");
```  
答案： TypeError  
static方法不能被子类继承， 只能通过类名```Chameleon.colorChange```来调用   

##### 下面代码的输出是什么?
```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);
console.log(sarah);
```  
答案：Person {firstName: "Lydia", lastName: "Hallie"} and undefined  
使用new是创建了一个Person的实例对象。**不使用new是在全局上运行Person里的语句**。```Person("Sarah", "Smith");```相当于```global.firstName ='Sarah'; global.lastName ='Smith'```