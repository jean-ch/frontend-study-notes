Block Formatting Context- 块格式化上下文   
内部子元素再怎么翻江倒海，翻云覆雨都不会影响外部的元素

####  创建一个BFC    
- float: left | right 
- position: absolute | fixed   
- display: table-cell | table-caption | inline-block, flex, inline-flex   
- overflow: hidden | auto | scroll   

最简单没有副作用的方法是overflow: hidden  

#### BFC盒子   
- BFC盒子中的内部元素会从上倒下垂直放置   

- BFC内部盒子们垂直方向上的margin由元素盒子本身决定而不是简单的两个相邻盒子的margin之和 

- float不和同级的BFC区域重合    
BFC中每个盒子的左边框都紧挨着BFC container的左边框  
包括float元素也会和左边框对齐，那么float元素就会和其他盒子的左边框重叠   
除非float覆盖的盒子是个独立的BFC，那么它的盒子会因为float元素的存在而缩小，不再与float重合  

- 计算BFC的高度时，float元素也参与计算    

##### BFC造成margin collapse   
盒子与盒子的垂直间距本应是两个盒子margin的和   
但是在BFC中间距是两个margin取较大的那一个   

##### BFC防止margin collapse  
解决办法是把下面一个盒子设成一个独立的BFC   

##### BFC包含float   
float的元素因为不在常规流里面，所以包含float元素的container是没有高度的   
可以把container设为一个BFC，这样container就会包含住float元素，从而回到常规流里面   

##### BFC防止文字环绕  
一个BFC里面float元素和他的下一个盒子是左边框对齐的，因为float元素将会把下一个元素覆盖掉。如果下个元素里面有文字的话，文字的line boxes会给float元素移位，从而达到文字环绕的效果   
如果不想要文字环绕，可以把包含文字的下一个元素设成一个BFC，这样整个盒子都会给float元素移位    