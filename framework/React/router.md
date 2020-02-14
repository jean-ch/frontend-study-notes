[阮一峰的入门教程](https://www.ruanyifeng.com/blog/2016/05/react_router.html)   
```
<Router history={hashHistory}>
  <Route path="/" component={App}>
    // IndexRoute 设定父 url 默认渲染的 component
    <IndexRoute component={Home}/>
    <Route path="/repos" component={Repos}/>
    <Route path="/about" component={About}/>
  </Route>
</Router>
```
App 的 this.props.children 是子组件

#### 表单

- browserHistory.push(path)  
- this.context.router.push(path)

#### 路由的钩子

- onLeave
- onEnter