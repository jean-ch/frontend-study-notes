#### 基本类型
- number
- boolean
- string
- Array: number[] / Array<number>
- Tupler: [number, number]
- enum: ```enum Color {Red, Green, Blue}; let c: Color = Color.Green```
- any: any 和 object 的区别是 any 上可以调用任意方法
- void: 通常定义函数的返回类型 ```function F: void() {}```
- null, undefined: 所有类型的子集，可以赋值给任意类型 ```let n: null = null; let u: undefined = undefined;```
#### 类型断言
- ```<string>val```
- ```val as string```
#### 指定类型
```let {a, b}: {a: string, b: number} = o;```
#### 接口
```
interface example {
  a: string;
  b?: string // 可选属性
  readonly c: number // 制度属性属性
  [propName: string]: any;
}
```