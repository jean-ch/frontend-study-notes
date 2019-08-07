```
function asyncPool(limit, array, fn) {
    let i = 0;
    let promises = [];
    let executing = [];
    function enqueue() {
        if (i === array.length) {
            return Promise.resolve();
        }

        let item = array[i++];
        let p = Promise.resolve().then(fn.bind(this, item, array));
        promises.push(p);

        let e = p.then(() => executing.splice(executing.indexOf(e), 1));
        executing.push(e);

        let sequence = Promise.resolve();
        if (executing.length >= limit) {
            sequence = Promise.race(promises);
        }

        sequence.then(enqueue);
    }

    return enqueue().then(() => Promise.all(promises));
}
```