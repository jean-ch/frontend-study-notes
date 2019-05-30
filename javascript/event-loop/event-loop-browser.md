#### browser中event loop流程: task -> micro task -> UI render 
- 从 task 队列（一个或多个）中选出最老的一个 task，执行它。  
- 执行 microtask checkpoint。简单说，会执行 microtask 队列中的所有 microtask，直到队列为空。如果 microtask 中又添加了新的 microtask，直接放进本队列末尾。  
- 执行 UI render 操作：  
	- 判断 document 在此时间点渲染是否会『获益』。浏览器只需保证 60Hz 的刷新率即可（在机器负荷重时还会降低刷新率），若 eventloop 频率过高，即使渲染了浏览器也无法及时展示。所以并不是每轮 eventloop 都会执行 UI Render  
	- 执行各种渲染所需工作，如 触发 resize、scroll 事件、建立媒体查询、运行 CSS 动画等等  
	- 执行 animation frame callbacks  
	- 执行 IntersectionObserver callback   
	- 渲染 UI  
![流程图](https://www.screencast.com/t/Fr9tNpAMPvd)  

#### event loop流程代码示例 
```
//步骤1： 开始执行首个 eventloop 的 task 阶段
console.log('A') // 步骤2：**输出 A**
setTimeout(() => {  // 步骤3：立刻将 callback（B） 放入 task 队列中
  console.log('B')
}, 0)
Promise.resolve().then(() => {  // 步骤4：立刻将 callback（C） 放入 microtask 队列中
  console.log('C')
}).then(() => {
  console.log('D')
})
console.log('E') // 步骤5：**输出 E**
// 步骤6：首个 eventloop 的 task 阶段执行完毕，开始执行 microtask，发现有一个 callback（C），执行之，**输出 C**，同时又将 callback（D）放入 microtask 队列中
// 步骤7：发现 microtask 队列不为空，执行 callback（D），**输出 D**
// 步骤8：microtask 队列为空，执行 UI render，（根据机器负荷等环境影响，综合浏览器策略，此步骤可能执行也可能不执行）
// 步骤9：首次 eventloop 结束。执行第二轮 eventloop，取出一个 task callback（B），执行之，**输出 B**
```  

#### micro task的执行时机  
把task栈中的第一个task拉到执行栈中执行完后就会进行micro task checkpoint
micro task的执行时机是见缝插针，尽可能早。只要javascript的执行栈为空，就执行micro task。因此一个event loop可以执行多次micro task  

##### Promise.resolve()
放在micro task的尾部，因此在本轮event loop结束时执行  

