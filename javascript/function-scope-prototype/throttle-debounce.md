### 节流 throttle 
规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。 
举例: 
- 监听滚动时间，比如是否滑到底部自动加载更多，用throttle来判断  
- 鼠标不断点击触发，比如mousedown(单位时间内只触发一次)  

实现方法： 
- 用闭包保存一个标记
- 在规定时间内出发，标记为false，不予执行
- setTimeout等待规定时间后把标记设为true   
```
function throttle(fn, interval = 300) {
    let canRun = true;
    return function () {
        if (!canRun) return;
        canRun = false;
        args = arguments
        setTimeout(() => {
            fn.apply(this, args);
            canRun = true;
        }, interval);
    };
}
```

### 防抖 debounce 
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。   
举例： 
- search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
- window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次 

实现方法：   
- 用闭包保存一个setTimeout的返回值   
- 每当有新的用户输入的时候调用debounce函数，把前一次的timeout id clear掉  
- 这样就能保证delay时间内如果用户还有在输入，不会执行ajax。只有指定间隔内没有输入时才会执行函数      
```
function ajax(content) { // 模拟一个api call
  console.log('ajax request ' + content)
}

function debounce(fun, delay) {
    let timer = null;
    return function () {
        const that = this;
        const args = arguments;
        clearTimeout(timer);
        timer = setTimeout((() => fun.apply(this, args)), delay)
    }
}

debounce(ajax, 500)(content);
```
带立即执行选项的防抖函数
```
function debounce(func, delay, immediate) {
	let timer = null;
	return function() {
        const that = this;
        const args = arguments;
        timer && clearTimeout(timer);
		if (immediate) {
			func.apply(this, arguments);
			setTimeout(() => timer = null, delay);
		} else { 
			setTimeout(() => func.apply(that, args), delay);
		}
	};
}
```
函数有返回值，因为在setTimeout里是异步返回所以用Promise封装 
```
function debounce(func, delay) {
    let timer = null;
    return function() {
        const that = this;
        const args = arguments;
        return new Promise((resolve, reject) =>
            timer && clearTimeout(timer);
            setTimeout(() => func.apply(that, args), delay);
        )
    }
}
```