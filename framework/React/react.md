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
1. change with mutation```arr[1] = 'change'```   
2. without mutation```let temp = arr; temp[1] = 'change'; setData(arr, temp);```
- 方便监听
- 优化reflow
- 用cancat代替push不会对原array产生mutation




- setState
？ 单向绑定和双向绑定    
？ 怎么处理state