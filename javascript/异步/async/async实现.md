```
function spawn(genF) {
	return new Promise((resolve, reject) => {
		const gen = genF();
		function step(nextF) {
			let next;
			try {
				next = nextF();
			} catch(e) {
				reject(e);
			}

			if (next.done) {
				resolve(next.value);
			}

			Promise.resolve(next.value).then((value) => {
				step(() => gen.next(value));
			}, (error) => {
				step(() => gen.throw(error));
			})
		}

		step(() => gen.next(undefined));
	})
}

async function fn(args) {}

function fn(args) {
	return spawn(function* () {})
}
```