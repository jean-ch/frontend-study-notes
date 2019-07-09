### BFC
Block Formatting Context- 块格式化上下文   
内部和外部元素不会互相影响

####  创建一个BFC    
- float: left | right 
- position: absolute | fixed   
- display: table-cell | table-caption | inline-block, flex, inline-flex   
- overflow: hidden | auto | scroll   

最简单没有副作用的方法是overflow: hidden  

#### BFC特性 
- BFC盒子中的内部元素会从上到下垂直放置   

- BFC子元素的margin发生重合

- BFC不与float重合 
BFC中每个盒子的左边框都紧挨着BFC container的左边框  
包括float元素也会和左边框对齐，那么float元素就会和其他盒子的左边框重叠   
除非float覆盖的盒子是个独立的BFC，那么它的盒子会因为float元素的存在而缩小，不再与float重合  

- 计算BFC的高度时，float元素也参与计算    

#### 应用
##### margin collapse   
相邻块状元素的垂直间距本应是两个盒子margin的和   
但是在BFC中，间距是两个margin取较大的那一个   

##### 防止margin collapse  
相邻元素设成独立的BFC  

##### 包裹float   
float的元素因为不在常规流里面，所以包含float元素的container是没有高度的   
可以把container设为一个BFC，这样container就会包含住float元素，从而回到常规流里面   

##### 防止float文字环绕  
一个BFC里面float元素和他的下一个盒子是左边框对齐的，因为float元素将会把下一个元素覆盖掉。如果下个元素里面有文字的话，文字的line boxes会给float元素移位，从而达到文字环绕的效果   
如果不想要文字环绕，可以把包含文字的下一个元素设成一个BFC，这样整个盒子都会给float元素移位    

##### 自适应的两栏布局
```
.left{
    float: left;
    width: 300px;
}
.right{
    overflow: hidden; //通过设置overflow: hidden来触发BFC特性
}
```

##### 清除内部浮动 

### IFC
Inline Formatting Contexts- 内联格式化上下文
- 高度由子元素的实际高度决定，不受垂直pardding/margin影响
- IFC中没有块级元素，插入div会隔开两个IFC

#### 应用
##### 水平居中
设置inline-block产生IFC  
text-align: center水平居中

##### 垂直居中
设置inline-block产生IFC  
vertical-align: middle垂直居中