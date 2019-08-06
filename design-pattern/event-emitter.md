#### 发布订阅
- eventPool维护event和callback列表之间的关系
- on：把callback加入对应event的callback列表
- off：移除callback
- emit：把event的所有callback拿出来执行
- once：只执行一次，所以包装一个一次性执行的callback（通过调用off达成只执行一次）放入callback列表中
```
class EventEmitter {
	constructor() {
		this._eventPool = {};
	}

	on(event, callback) {
		this._eventPool[event] ? this._eventPool[event].push(callback) : this._eventPool[event] = [callback];
		return this;
	}

	off(event) {
		if (this._eventPool[event]) {
			delete this._eventPool[event];
		}

		return this;
	}

	emit(event, ...args) {
		this._eventPool[event].forEach(job => job(...args));
		return this;
	}

	once(event, callback) {
		this.on(event, (...args) => {
			callback(...args);
			this.off(event);
		})

		return this;
	}
}
```

#### 发布订阅和观察者模式之间的区别
- 观察者模式Subject和Observers之间彼此互相知道（松散耦合）
- 发布订阅模式借用信息中介中间件，处理不同组件之间的信息交流，组件之间互不知道对象存在（解耦）