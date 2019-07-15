```
class Promise(executor) {
  constructor(handle) {
    if (typeof handle != 'function') {
        throw new Error('promise only accept a function parameter');
    }

    this.status = 'pending';
    this.value = undefined;
    this.fulfilledQueue = [];
    this.rejectedQueue = [];
    try {
        handle(this.resolve.bind(this), this.reject.bind(this));
    } catch(e) {
        this.reject(e);
    }
  }

  resolve(val) {
    if (this.status != 'pending') {
        return;
    }

    const run = () => {
        this.status = 'fulfilled';
        this.value = val;
        let job;
        while (job = this.fulfilledQueue.shift()) {
            job(this.value);
        }
    }

    setTimeout(() => run(), 0); //为支持同步promise，这里采用异步调用
  }

  reject(val) {
    if (this.status != 'pending') {
        return;
    }

    const run = () => {
        this.status = 'rejected';
        this.value = val;
        let job;
        while (job = this.fulfilledQueue.shift()) {
            job(this.value);
        }
    }

    setTimeout(() => run(), 0); //为支持同步promise，这里采用异步调用
  }

  then(onFulfiled, onRejected) {
    return new Promise((onFulfilledNext, onRejectedNext) => { //满足链式调用
        let fulfilled = (this.value) => {
            try{
                if (typeof onFulfiled != 'function') {
                    onFulfilledNext(this.value); //值穿透
                } else {
                    let res = onFulfiled(this.value);
                    if (res instanceof Promise) {
                        res.then(onFulfilledNext, onRejectedNext); //返回的是promise，等待状态改变后执行下一个then的回调
                    } else {
                        onFulfilledNext(res);//否则将返回结果作为参数传给下个then的回调
                    }
                }
            } catch(e) {
                onRejectedNext(e);
            }
        }

        let rejected = (err) => {
            try {
                if (typeof onRejected != 'function') {
                    onRejectedNext(err);
                } else {
                    let res = onRejected(err);
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
    }

    switch(this.status) {
        case 'pending': 
            this.fulfilledQueue.push(onFulfiled);
            this.rejectedQueue.push(onRejected);
            break;
        case 'fulfilled':
            onFulfiled(this.value);
            break;
        case 'rejected':
            onRejected(this.value);
            break;
    }
  }
}
```

[详版指路](https://juejin.im/post/5b83cb5ae51d4538cc3ec354)