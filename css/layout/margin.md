#### margin auto
用来居中元素 
对float和absolute元素无效，因为这俩不在正常流里面   

#### 负margin     
  - top/left往外推   
  - bottom/right往里拉    
应用：   
- 边框去重- 不希望tab元素和父元素都加上边框    
  - margin-left: -1px(border width);   
  - 父元素 overflow: hidden; 隐藏溢出部分  
- [左右布局](https://codepen.io/jeancccccc/pen/qzEYOZ)   
```
.left {
  height:400px;
}
.right {
  margin-top:-400px 
  margin-left: 100px;
}
```   
  - right元素本来应该在下一行，给它设置margin-top: -400px(left元素的高度)，把right元素推到和left同一行（如果right的height没有给定400px的话，要想left right等高left的margin-bottom要设置400px，不然left元素占两行）  
  - right元素的左边因为要空出left元素，所以margin-left要设成100px(left元素的宽度)  

#### margin collapse   
两个**块级**元素会发生**垂直**方向上的margin collapse  
避免margin collapse   
- 改变块级元素- 设成float  
- 设置成BFC   
...