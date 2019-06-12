#### margin auto
用来居中元素  

#### 负margin     
margin参考线：  
  - top/left以相连元素的边界作为参考线     
  - bottom/right以本元素的边界作为参考线   
margin为负时参考线的运动：  
  - top/left将相连元素的边界往外推   
  - bottom/right将本元素的边界往里拉    
应用：   
- 边框去重- 不希望tab元素和父元素都加上边框    
  - margin-left: -1px(border width);   
  - 父元素 overflow: hidden; 隐藏溢出部分  
- [左右布局](https://codepen.io/jeancccccc/pen/qzEYOZ)      
  - right元素本来应该在下一行，给它设置margin-top: -400px(left元素的高度)，把right元素推到和left同一行（如果right的height没有给定400px的话，要想left right等高left的margin-bottom要设置400px，不然left元素占两行）  
  - right元素的左边因为要空出left元素，所以margin-left要设成100px(left元素的宽度)  

#### margin collapse   
- only top/bottom margins collapse, won't happen on left/right margins