#### float
从原本的流布局中拿掉- 所以其他的元素会占据float元素原本在流布局中的位置   
拿掉之后float确依然影响流布局- 表现在其他元素占据了float元素的位置但是文字确实环绕float元素的（这是和position absolute的区别）

#### 清除浮动
##### Problem
```
<div class="outer">
    <div class="div1">1</div>
    <div class="div2">2</div>
    <div class="div3">3</div>
</div>
<div class="outer">
    <div class="div1">1</div>
    <div class="div2">2</div>
    <div class="div3">3</div>
</div>
```
没有给最外层的DIV.outer 设置高度，里层的三个浮动元素撑不起来父元素的高度

##### 添加新的元素,应用 clear：both
```
<div class="outer">
    <div class="div1">1</div>
    <div class="div2">2</div>
    <div class="div3">3</div>
    <div class="clear"></div>
</div>
.clear{clear:both; height: 0; line-height: 0; font-size: 0}
```
##### 父元素overflow: auto
overflow: visible是不能够清除浮动的
```
.outer{
    overflow: auto; 
}
```

##### 用:after插入一个清除浮动的元素
```
.outer:after {
  clear:both;
  content:'.';
  display:block;
  width: 0;
  height: 0;
  visibility:hidden;
}
```