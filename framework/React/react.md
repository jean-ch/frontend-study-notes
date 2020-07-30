- JSX
  - {变量}
  - 驼峰命名 class -> className, tabindex -> tabIndex
  - 对象
    ```
    const element = (
      <h1 className="greeting">
        Hello, world!
      </h1>
    );
    ```
    ```
    const element = React.createElement(
      'h1',
      {className: 'greeting'},
      'Hello, world!'
    );
    ```
- 组件
  - 只有 props 的组件
    ```
    function f() { 
      return jsx;
    }
    ```
  - 有 state 的组件
    ```
    class F extends React.component { 
      render() {
        return jsx;
      }
    }
    ```

- createElement

##### change without mutation的一点启发
1. change with mutation  ```arr[1] = 'change'```   
2. without mutation ```let temp = arr; temp[1] = 'change'; setData(arr, temp);```
- 方便监听
- 优化reflow
- 用cancat代替push不会对原array产生mutation




- setState
？ 单向绑定和双向绑定    
？ 怎么处理state

#### 生命周期
- mount: 加载 Dom
  - constructor()
  - render()
  - componentDidMount()： 设置计时器，调用 ajax 请求
- Update: 更新 Dom
  - getDerivedStateFromProps(nextProps, prevState): 根据当前的 props 来更新组建的 state， 每个 render 都会调用此方法。适用于触发一些回调，如动画或页面跳转等
  - shouldComponentUpdate(nextProps, nextState): 组件接收新的props或state时调用，return true 就会更新dom， return false就能阻止更新，主要用于性能优化(部分更新)
  - getSnapshotBeforeUpdateprevProps, prevState): 在元素被渲染并写入DOM之前调用，使得组件能在发生更改之前从 DOM 中捕获一些信息，此生命周期的任何返回值将作为参数传递给 componentDidUpdate(). 想获得聊天窗口中的滚动位置，可以通过这个方法获取信息


  - render()
  - componentDidUpdate()
- unmount: 移除 Dom
  - componentWillUnmount()

#### setState
- 异步更新接收函数
```
// wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});

// correct
this.state((state, props) => ({
  counter: state.counter + props.increment
}))
```