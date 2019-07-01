#### postMessage + onMessage
子页面 postMessage
```
var ifr=window.parent;
  var targeOrigin='http://localhost:63342/reactWork';
  ifr.postMessage("asa1111",targeOrigin);
```
父页面监听message事件
```
<iframe style="display: none" id="ifr" src="http://localhost:63342/reactWork/index2.html"></iframe>
<script type="text/javascript">
     window.addEventListener('message',function (event) {
         console.log(event)
         if(event.origin=='http://localhost:1234'){
             console.log(event.data)  //会输出event的数据
         }
     },false)
</script>
```