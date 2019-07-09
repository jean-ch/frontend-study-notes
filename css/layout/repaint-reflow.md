#### 浏览器的渲染过程
- 解析
  - 解析HTML生成DOM Tree
  - 解析CSS生成CSS规则树
  - 解析JS脚本，准备通过DOM API和CSSOM API来操作DOM Tree和CSS Rule Tree
- 通过DOM Tree和CSS Rule Tree来构造Rendering Tree
  - CSS规则树的作用是完成匹配，把CSS规则附加到Rendering Tree的对应Frame(对应DOM节点)上
  - 计算每个DOM节点的位置

#### 回流和重绘
- 重绘repaint——屏幕的一部分要重画，比如某个CSS的背景色变了。但是元素的几何尺寸没有变
- 回流reflow——意味着元件的几何尺寸变了，我们需要重新验证并计算render tree。reflow 会从\<html\>这个root frame开始递归往下，依次计算所有的结点几何尺寸和位置

#### 造成reflow
- 页面渲染初始化
- DOM结构变化：增加、删除、修改、移动DOM结点
- render树变化：修改CSS样式如减少padding，修改网页的默认字体
- 窗口resize事件触发，或窗口滚动
- 获取某些属性，引发回流 
  - offsetTop, offsetLeft, offsetWidth, offsetHeight
  - scrollTop/Left/Width/Height
  - clientTop/Left/Width/Height
  - width,height
  - 调用了getComputedStyle(), 或者IE的currentStyle

#### 减少reflow
- 避免逐项更改样式。最好一次性更改style属性，或者将样式列表定义为class并一次性更改class属性
- 避免多次读取offsetLeft等属性。无法避免则将它们缓存到变量
- 不要使用table布局
- 将复杂的元素绝对定位或固定定位，使它脱离文档流。否则回流代价十分高
- 把DOM离线后修改
##### display:none
先把DOM给display:none(一次reflow)，修改完成后再display:block(第二次reflow)
##### innerHTML
```
const ul = document.getElementById('content')
const lists = ['a', 'b', 'c', 'd']
const childElementString = lists.map(list=>`<li>${list}</li>`).join('')
ul.innerHTML = ul.innerHTML + childElementString //一次reflow
```
##### DocumentFragment
DocumentFragment是保存在内存中的虚拟DOM
```
const parentNode = document.getElementById('content')
const lists = ['a', 'b', 'c', 'd']
const fragment = document.createDocumentFragment
lists.forEach(text=>{  // 不用DocumentFragment这里会发生4次reflow
	const li = document.createElement('li')
	li.textContent = text
	fragment.appendChild(li)
})
parentNode.appendChild(fragment) //一次reflow
```

##### display：none和hidden的区别
display:none会触发reflow，而visibility:hidden只会触发repaint

#### innerHTML和document.write的区别
document.write直接写入页面的文档流
innerHTML是DOM页面元素的一个属性
- 更精准：documnet.write重写整个页面，innerHTML可以精确到具体某个元素
- 更高效：documnet.write重绘整个页面，innerHTML只重绘元素所在的这一部分