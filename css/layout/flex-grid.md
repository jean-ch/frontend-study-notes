### flex
#### align-items
- flex-start: 垂直靠上 
- flex-end: 垂直靠下
- center: 垂直居中
- stretch: 垂直抻开
- baseline: 垂直靠中上

#### align-content 
有多根轴线时， 所有子元素在container里相对垂直方向的排列方法 
- flex-start: 整体区域靠上 
- flex-end: 整体区域靠下 
- center: 整体区域垂直居中
- stretch: 整体区域抻满垂直方向
- space-between
- space-around 

#### align-self 
设置在子元素上，允许单个元素在垂直方向上有不一样的对齐方式，覆盖父元素的align-items属性 
- auto 继承父元素的align-items
- flex-start, flex-end, center, baseline, stretch

#### justify-content  
- flex-start: 水平靠左
- flex-end: 水平靠右 
- center: 水平居中
- space-between： 间隔
- space-around: 两边也间隔

### grid  
#### grid-auto-flow
相当于flex的flex-direction   

#### align-items
设置单元格内容的垂直位置  
start | end | center | stretch

#### align-content
整个内容区域的垂直位置   
start | end | center | stretch | space-around | space-between | space-evenly

#### align-self
设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致

#### justify-items 
设置单元格内容的水平位置  
start | end | center | stretch

#### justify-content
整个内容区域在容器里面的水平位置  
start | end | center | stretch | space-around | space-between | space-evenly

#### justify-self
设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致

### box-alignment  
#### 垂直方向 
- align-items 
- align-self
- align-content

#### 水平方向 
- justify-items (flex没有)
- justiry-self (flex没有)
- justify-content