#### static  
默认，元素处于正常的文档流中   

#### relative   
仍然在正常文档流中，根据top left根据文档流中正常位置做出偏移   

#### absolute  
从正常文档流中拿出来。   
绝对位置的偏移是针对最近的**非static**的祖先做出的 
因此要想absolute针对某个父元素做出偏移，这个父元素的位置大多是relative的(不设置top left等relative的位置和static一样)      

#### fixed    
跟absolute一样从正常文档流中拿出来了  
但是他的相对便宜是针对视图而不是某个元素   
因此fixed的特点是page scroll的时候fixed元素始终处于viewpoint的同一位置    

#### sticky
sticky的元素本来出现在正常的文档流里   
当页面滚动的时候sticky的元素随之滚动，直到元素相对于viewpoint的位置满足设置的left right top等偏移量，它就针对viewpoint固定下来，不再随页面滚动而产生位置变化了   
因此sticky要求**必须要设置left right top等至少其中一个**  
并且由于基于滚动效果，sticky元素的**父元素overflow属性必须是visible**   