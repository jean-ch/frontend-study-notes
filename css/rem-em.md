#### px
针对像素的绝对大小

#### em
相对父级字体大小  
- 默认字体大小16px。1em = 16px，12px=0.75em，10px=0.625em

- 用百分比简易设置```body { font-size: 62.5%; }```：font-size = 10px

- 父级字体倍数的多重嵌套：  
在父级中声明了字体大小为1.2em，那么在声明子元素的字体大小时设置1em才能和父级元素内容字体大小一致，而不是1.2em（避免 1.2*1.2=1.44em）, 因为此em非彼em
eg:
  ```
  <span>Outer <span>inner</span> outer</span>

  body { font-size: 62.5%; }
  span { font-size: 1.6em; }
  // 外层 <span> 为 body 字体 10px 的 1.6倍 = 16px，内层 <span> 为外层内容字体 16px 的 1.6倍 = 25px
  ```

#### rem
相对于html根标签的font-size属性值来计算   
- 没有多重嵌套的烦恼
  ```
  <span>Outer <span>inner</span> outer</span>

  body { font-size: 62.5%; }
  span { font-size: 1.6rem; }
  // 内外 <span> 的内容均为 16px
  ```