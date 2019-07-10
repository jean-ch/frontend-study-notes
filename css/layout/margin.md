#### margin auto
用来居中元素 
对float和width,height不确定的absolute元素无效，因为这俩不在正常流里面   

#### 负margin     
  - top/left往外推   
  - bottom/right往里拉    
应用：   
- 边框去重- 不希望tab元素和父元素都加上边框    
  - margin-left: -1px(border width);   
  - 父元素 overflow: hidden; 隐藏溢出部分  
- [左右布局](https://codepen.io/jeancccccc/pen/qzEYOZ)   
  - right元素本来应该在下一行，给它设置margin-top: -400px(left元素的高度)，把right元素推到和left同一行（如果right的height没有给定400px的话，要想left right等高left的margin-bottom要设置400px，不然left元素占两行）  
  - right元素的左边因为要空出left元素，所以margin-left要设成100px(left元素的宽度)  
  ```
  .left {
    height:400px;
  }
  .right {
    margin-top:-400px 
    margin-left: 100px;
  }
  ```   

#### margin collapse   
##### 发生条件
- 垂直方向
- 块级元素
- 相邻： 没有被非空内容、padding、border 或 clear 分隔开
  - 一个元素的margin-top会和它普通流中的第一个子元素(非浮动元素等)的 margin-top 相邻
  - 只有在一个元素的height是auto的情况下，它的margin-bottom才会和它普通流中的最后一个子元素(非浮动元素等)的margin-bottom相邻
- 处于同一个BFC

##### 避免margin collapse   
- 改变块级元素- 设成float, inline-block, position: absolute
- BFC不和其子元素发生margin折叠  
- 相邻元素设置成单独的BFC   