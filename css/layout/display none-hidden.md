#### display: none和visibility: hidden的区别
- display:none触发重排，visibility:hidden触发重绘
- display不是继承属性，而visibility是继承属性
- display:none不在render tree里，而visibility：hidden在render tree里占一个空白位
- display: none不支持transition渐变，而hidden支持
- 都不能触发事件

#### opacity： 0
- 不触发回流和重绘
- 子元素opacity:1并不能改变隐藏
- 仍然能够触发事件

#### 其他隐藏元素的方法
- width, height, padding, margin, border都设为0，此外还要将overflow设为hidden用于隐藏子元素