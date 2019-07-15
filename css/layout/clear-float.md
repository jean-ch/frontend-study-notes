#### float
- 从流布局中拿掉
- 产生包裹性- div的宽度本来是占一整行，设成float之后对内容产生包裹性，宽度自适应
- 高度欺骗- 父元素的高度不会被float元素的高度撑起来
- 被inline元素环绕- 文字环绕

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
##### 父元素BFC
父元素overflow: auto | hidden设置成BFC，父元素的高度将被float撑起来，达到清除浮动的效果
```
.outer{
    overflow: auto; 
}
```

##### 用::after插入一个清除浮动的元素
```
.outer::after {
  clear:both;
  content:'.';
  display:block;
  width: 0;
  height: 0;
  visibility:hidden;
}
```

##### .clearfix类
```
.clearfix:before,.clearfix:after {
  display: table;
  content: " "
}

.clearfix:after {
  clear: both
}
```
