generator有两个特点  
1. 他是一个返回iterator对象的函数  
	- 因此可以用来包装返回遍历数组   
2. 因为yield关键字的存在，所以generator内部能够管理状态基  
另外generator提供两个功能   
1. 通过yiled往外部传参，通过next往generator里传参  
2. 通过外部throw error能够在generator内部catch到，generator里throw的error也能在外部catch到       

#####  遍历二叉树   
```
function Tree(left, label, right) {
  this.left = left;
  this.label = label;
  this.right = right;
}

// 下面是中序（inorder）遍历函数。
// 由于返回的是一个遍历器，所以要用generator函数。
// 函数体内采用递归算法，所以左树和右树要用yield*遍历
function* inorder(t) {
  if (t) {
    yield* inorder(t.left);
    yield t.label;
    yield* inorder(t.right);
  }
}

// 下面生成二叉树
function make(array) {
  // 判断是否为叶节点
  if (array.length == 1) return new Tree(null, array[0], null);
  return new Tree(make(array[0]), array[1], make(array[2]));
}
let tree = make([[['a'], 'b', ['c']], 'd', [['e'], 'f', ['g']]]);

// 遍历二叉树
var result = [];
for (let node of inorder(tree)) {
  result.push(node);
}

result
// ['a', 'b', 'c', 'd', 'e', 'f', 'g']
```

##### 异步自动流程管理  
```
function* g() {
	// ...
}

function run(fn) {
	var gen = fn();

	function next(err, data) {
		var result = gen.next(data);
		if (!result.done) {
			result = gen.next(result.value);
			next();
		}
	}

	next();
}
```