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
  //私有变量和私有方法
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
  o.getComponentCount = function() {
    return components.length;
  };
    
  o.registerComponent = function() {
    if (typeof component == 'object') {
      components.push(component);
    }
  };

  return o;
}
```
节约资源，使用时才初始化
```
var Singleton = (function () {
    var instantiated;
    function init() {  //用另一个构造函数来初始化
        /*这里定义单例代码*/
        return {
            publicMethod: function () {
                console.log('hello world');
            },
            publicProperty: 'test'
        };
    }

    return {
        getInstance: function () {
            if (!instantiated) {
                instantiated = init();
            }
            
            return instantiated;
        }
    };
})();

/*调用公有的方法来获取实例:*/
Singleton.getInstance().publicMethod();
```