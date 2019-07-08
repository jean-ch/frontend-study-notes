#### 事件的触发顺序: 
- 对于非target节点则先执行捕获在执行冒泡
- 对于target节点则是先执行先注册的事件，无论冒泡还是捕获

#### 事件代理  
```
<!-- 监听多个列表 -->
<ul id="list">
  <li id="item1">item1</li>
  <li id="item2">item2</li>
  <li id="item3">item3</li>
  <li id="item4">item4</li>
</ul>
```
```
<!-- 如果在每个li上放置事件监听，添加或删除li都会不停的更改操作 -->
window.onload=function(){
  var ulNode=document.getElementById("list");
  var liNodes=ulNode.childNodes||ulNode.children;
  for(var i=0;i<liNodes.length;i++){
    liNodes[i].addEventListener('click',function(e){
      alert(e.target.innerHTML);
    },false);
  }
}
```
```
<!-- 使用事件代理，在ul上放一个监听 -->
(function(){
    var color_list = document.getElementById('color-list');
    color_list.addEventListener('click',showColor,false);
    function showColor(e){
        var x = e.target;
        if(x.nodeName.toLowerCase() === 'li'){
            console.log('The color is ' + x.innerHTML);
        }
    }
})();
```

##### addEventListener和onclick的区别
- onclick只能给一个对象添加监听，addEventListener可以同时给多个对象添加监听
- addEventListener可以指明是冒泡还是捕获