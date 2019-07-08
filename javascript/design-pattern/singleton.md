用对象字面量来创建单例对象
```
var singleton = {
  name: value,
  method: function() {}
}
```
模块模式添加私有变量和特权方法
```
var singleton = function() {
  //私有变量
  var components = new Array();

  //初始化
  components.push(new BaseComponent());

  //将公共接口用字面量返回
  return {
    getComponentCount: function() {
      return components.length;
    },
    registerComponent: function() {
      if (typeof component == 'object') {
        components.push(component);
      }
    }
  }
}
```
返回特定类型的实例
```
var singleton = function() {
  var components = new Array();
  components.push(new BaseComponent());

  //创建特定类型的对象
  var o = new BaseComponent();

  //添加公共接口
  app.getComponentCount = function() {
    return components.length;
  };
    
  app.registerComponent = function() {
    if (typeof component == 'object') {
      components.push(component);
    }
  };

  return o;
}
```