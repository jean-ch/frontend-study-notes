### position absolute + top 50% + margin -(1/2)height  
必须知道被居中元素的尺寸
```
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
    width: 150px;
    height: 100px; // 定高
    background: orange;
    position: absolute;
    top: 50%;
    margin: -50px 0 0 0;  // 负高度的一半
}
```

### position absolute + margin auto + (top == bottom)
```
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
    width: 200px;
    height: 100px;  // height可以不用设了，高度就是子元素撑起来的高度，当然要是没有子元素那就没有高度
    background: orange;
    position: absolute;
    top: 0;  // top和bottom设成相同的任意值都可以，这里设成0
    bottom: 0;
    margin: auto;
}
```

### 父元素height不定，上下padding设置相等 
```
#box {
    width: 300px;
    background: #ddd;
    padding: 100px 0; //父元素的高度靠子元素和padding撑起来
}
#child {
    width: 200px;
    height: 100px;
    background: orange;
}
```

### line-height == container height 对单行文本垂直居中  
```
<div id="box">test vertical align</div>
#box{
    width: 300px;
    height: 300px;
    background: #ddd;
    line-height: 300px; // line-height必须等于height
    text-align: center; // text-align定义水平对齐方式，加上text-align就会垂直水平都居中
}
```

### (line-height == height) + 子元素vertical-align middle 对图片垂直居中  
```
#box{
    width: 300px;
    height: 300px;
    background: #ddd;
    line-height: 300px;
}
#box img {
    width: 200px;
    height: 200px;
    vertical-align: middle;
}
```
  
### table-cell + vertical-align middle   
```
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: table;
}
#child {
    display: table-cell;
    vertical-align: middle;
}
```
#### vertical-align

vertival-align只对inline, inline-block, table-cell元素起作用   
inline-block元素: 图片，button，单复选框，单行/多行文本      
同样的text-align也只对inline-block元素水平居中 

### flex
#### flex + align-items center
```
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    align-items: center;
}
```
或改变flex-direction,用justify-content
```
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
```
#### flex + margin-auto

### grid 
```
<div id="box">
    <div></div> // 借助了上下两个辅助div实现三栏布局
    <div class="two">target item</div>
    <div></div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr; // 要求.two宽度已知也可以写成1fr 211px 1fr
}
.two {
    background: orange;
}
```