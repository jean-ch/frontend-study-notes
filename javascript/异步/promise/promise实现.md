- constructor
    - constructor里主要维护_status和_value
    - constructor里维护的_fulfilledQueue和_rejectedQueue是用来存放状态改变后需要执行的函数
- _resolve, _reject
    - _resolve和_reject方法需要支持同步的promise，所以用异步setTimeout包装了一下
    - _resolve如果传入resolve的是promise的对象，要等这个promise对象状态改变后当前promise才能改变
- then, catch
    - then和catch都返回promise
    - then的回调函数参数有以下三种情况
        - 不是函数而是值，会发生值穿透
        - 函数返回promise，则要等待这个promise状态变化
        - 函数返回值，把值传递给链式调用的下个回调函数
    - catch相当于onFulfilled为undefined的then方法
- resolve, reject, all, race
    - resolve的参数如果是promise，直接返回这个promise，否则包装一个promise，并把参数传给promise的resolve
    - resolve, reject, all, race都是promise的static方法
    - Promise.all和Promise.race接收的都是遍历器对象

```
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';
const isFunction = variable => typeof variable === 'function';
class Promise {
	// handle: (resolve, reject) => {}
	constructor(handle) {
		if (!isFunction(handle)) {
			throw new Error('Promise must accept a function as parameter');
		}

		this._status = PENDING;
		this._value = undefined;

		this._fulfilledQueue = [];
		this._rejectedQueue = [];

		try {
			handle(this._resolve.bind(this), this._reject.bind(this));
		} catch (e) {
			this._reject(e);
		}
	}

	_resolve(val) {
		const run = () => {
			if (this._status !== PENDING) {
				return;
			}

			const runFulfilled = (val) => {
				let job;
				while (job = this._fulfilledQueue.shift()) {
					job(val);
				}
			}

			const runRejected = (err) => {
				let job;
				while (job = this._rejectedQueue.shift()) {
					job(val);
				}
			}

			// 如果传入resolve的是promise对象，要等待这个promise对象状态改变
			if (val instanceof Promise) {
				val.then(value => {
					this._value = value;
					this._status = FULFILLED;
					runFulfilled(value);
				}, error => {
					this._value = error;
					this._status = REJECTED;
					runRejected(error);
				})
			} else {
				this._value = val;
				this._status = FULFILLED;
				runFulfilled(val);
			}
		}

		// 为支持同步promise，这里采用异步调用
		setTimeout(run, 0);
	}

	_reject(e) {
		if (this._status !== PENDING) {
			return;
		}

		const run = () => {
			this._status = REJECTED;
			this._value = e;
			let job;
			while (job = this._rejectedQueue.shift()) {
				job(e);
			}
		}

		setTimeout(run, 0);
	}

	then(onFulfilled, onRejected) {
		const {_value, _status} = this;

		return new Promise((onFulfilledNext, onRejectedNext) => {
			let fulfilled = value => {
				try {
					// 值穿透
					if (!isFunction(onFulfilled)) {
						onFulfulledNext(value);
					} else {
						let res = onFulfulled(value);
						// 如果返回的是promise对象，需要等待promise状态改变后再执行下一个回调
						if (res instanceof Promise) {
							res.then(onFulfilledNext, onRejectedNext);
						} else {
							onFulfilledNext(res);
						}
					}
				} catch(e) {
					onRejectedNext(e);
				}
			}

			let rejected = error => {
				try {
					if (!isFunction(onRejected)) {
						onRejectedNext(error);
					} else {
						let res = onRejected(error);
						if (res instanceof Promise) {
							res.then(onFulfilledNext, onRejectedNext);
						} else {
							onRejectedNext(res);
						}
					}
				} catch(e) {
					onRejectedNext(e);
				}
			}

			switch (_status) {
				case PENDING:
					this._fulfilledQueue.push(fulfilled);
					this._rejectedQueue.push(rejected);
					break;
				case FULFILLED:
					fulfilled(_value);
					break;
				case REJECTED:
					rejected(_value);
					break;
			}
		});
	}

	catch(onRejected) {
		return this.then(undefined, onRejected);
	}

	static resolve(value) {
		if (value instanceof Promise) {
			return value;
		} else {
			return new Promise(resolve => resolve(value));
		}
	}

	static reject(value) {
		return new Promise((resolve, reject) => reject(value));
	}

	static all(promises) {
		return new Promise((resolve, reject) => {
			// 先把iterable对象转成数组
			promises = Array.from(promises);
			if (promises.length === 0) {
				return resolve([]);
			}
			let values = [];
			let count = 0;
			for (let [index, promise] of list.entries()) {
				this.resolve(promise).then(res => {
					values[index] = res;
					if (++index === promises.length) {
						resolve(values);
					}
				})
			}
		}, err => {
			reject(err);
		})
	}

	static race(promises) {
		return new Promise((resolve, reject) => {
			for (let promise of promises) {
				this.resolve(promise).then(res => {
					resolve(res);
				}, error => {
					reject(error);
				})
			}
		});
	}
}
```

[详版指路](https://juejin.im/post/5b83cb5ae51d4538cc3ec354)