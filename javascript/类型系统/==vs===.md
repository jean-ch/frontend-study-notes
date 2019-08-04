#### == vs ===
- == 相等运算符，在比较不同类型的数据时，会先将数据进行类型转换再做比较
- === 严格运算符，比较熟知和数据类型    
- NaN
  - NaN !== NaN      
  - 判断NaN可以用Object.is```Object.is(NaN, NaN) === true```

#### ==
```null == undefined```