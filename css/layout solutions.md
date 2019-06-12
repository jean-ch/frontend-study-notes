#### 两列布局  
##### left自适应宽度  
1. [position: absolute + float: right](https://codepen.io/jeancccccc/pen/GbgMXK)    
- 拿一个container包left， right两个元素   
- 两列布局： 
  - left元素设置 position: absolute
  - right元素设置 position: absolution + float: right    
  - container元素因为子元素要设置absolute, 所以container元素要设置 position: relative   
- left自适应宽度
  - container定宽
  - right定宽
  - left定宽- 计算得到的container宽度 - right宽度 - 边界
- container下面还有占据整行的元素，要清掉浮动效果 clear: right/both。因为right设置了float    

2. [flex](https://codepen.io/jeancccccc/pen/JQoOPw)    
- 两列等高布局： container上设置display: flex    
- left自适应宽度     
  - container定宽  
  - right定宽   
  - left上设置 flex: 1   

#### 三列布局   
##### 高度已知，left right两栏宽度固定，center栏宽度自适应  
1. [float](https://codepen.io/jeancccccc/pen/MMYOeV)   
- 三列布局     
  - left，right上各自设置float: left/right     
  - HTML中left和right放在center的前面。这样center在left和right同一行进行渲染（虽然看上去是填满了left和right中间的空间，但其实center占据了整整一行，只有文字会wrap around float元素）。假如HTML中right放在center元素的后面进行渲染，那么center已经占据了一整行，right在本来的流模型中只能在下面一行进行渲染，加上float: right之后也只能处于下面一行的right位置      
- center宽度自适应： center占满一整行，看上去好像填满了left和right中间的位置     
- 缺点： Dom节点顺序错误- right dom得放在center dom之前

2. [float + absolute](https://codepen.io/jeancccccc/pen/jjEYLR)  
- 主要解决单用float时center其实是填满了一整行，占用了left和right元素位置的问题  
  - 给center设置上position: absolute，且left和right各自等于left和right元素的宽度   
  - container父元素设置上position: relative。不然center absolute是相对页面的，高度就不受container控制了    

3. [absolute](https://codepen.io/jeancccccc/pen/pXvpGv?editors=1100#0)   
- left, center, right在dom中按顺序排列    
- 都设置position: absolute  
- left上设置left: 300px, right上设置right: 300px, 中间设置left: 300px, right: 300px   

4. [负margin](https://codepen.io/jeancccccc/pen/KjwRmY)  
- left定高定宽不用设置   
- center充分利用了负margin  
  - 首先用margin-top: -400px把center推上去一行        
  - 然后用margin-right： 300px把right的位置空出来    
  - margin-bottom就有点tricky了。这里因为定好了height，所以margin-bottom设成0就好了。假如没有定高，margin-bottom要设成400px才能使移动过后的center和之前一样高。但如果这样的话，第二行一整行都是center的margin-bottom，那么right就还在第三行。如果center的margin-bottom是0的话，center就渲染到了第二行   
  - 最后margin-left: -300px把left的位置空出来  
- right利用负margin + float  
  - 首先用margin-top: -400px把right推上第一行    
  - 因为right是定高的，所以只要设置float: right 就可以让right处于最右  
* 这个demo没有设置其他的margin， border等值，否则负margin要考虑这些值  

5. [flex](https://codepen.io/jeancccccc/pen/zVxjVN)   
- container上设置display: flex    
- 自适应宽度那一列设置flex: 1   

6. [table](https://codepen.io/jeancccccc/pen/JQoZRM)   
- 三列布局：   
  - container上设置display: table   
  - 每一列上设置display: table-cell    
- center宽度自适应： 在container上设置width: 100%，把整个container撑起来，从而迫使center也撑起来使table的content达到width 100%   

7. [grid](https://codepen.io/jeancccccc/pen/OePEjK)
- container上设置display: grid, grid-template-columns: 300px auto 300px定义left right宽度和center自适应    
