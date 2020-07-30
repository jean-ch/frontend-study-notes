- 动态参数  
```<a v-bind:[attributeName]="url"> ... </a>```
- 计算属性的setter  
  ```
  computed: {
    fullName: {
      // getter
      get: function () {
        return this.firstName + ' ' + this.lastName
      },
      // setter
      set: function (newValue) {
        var names = newValue.split(' ')
        this.firstName = names[0]
        this.lastName = names[names.length - 1]
      }
    }
  }
  ```  
- 动态切换class/style    
```<div v-bind:class="{ active: isActive }"></div>```  
isActive决定class中active属性是否存在  
```<div v-bind:class="[{ active: isActive }, errorClass]"></div>```  
数组语法  
- key  
避免元素复用  
v-for时使用key  
- 对象动态添加响应式属性  
```vue.$set()```  
  ```
  vm.userProfile = Object.assign({}, vm.userProfile, {
    age: 27,
    favoriteColor: 'Vue Green'
  })
  ```  
- v-for使用值范围   
```<span v-for="n in 10">{{ n }} </span>```  
- prop    
传入数组，布尔     
  ```
  <blog-post v-bind:likes="42"></blog-post> 
  <blog-post v-bind:is-published="false"></blog-post>   
  <blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>
  ```  
  ```
  <blog-post v-bind="post"></blog-post>
  // 等价于
  <blog-post v-bind:id="post.id" v-bind:title="post.title"></blog-post>
  ```
- prop, data  
prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用  
  ```
  props: [
    'initialCounter'
  ],
  data: function () {
    return {
      counter: this.initialCounter
    }
  }
  ```
- v-model  

##### 生命周期

###### created
在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发updated函数。
可以做一些初始数据的获取
在当前阶段无法与Dom进行交互，如果非要想，可以通过vm.$nextTick来访问Dom。

##### mounted
在挂载完成后发生，在当前阶段，真实的Dom挂载完毕，数据完成双向绑定，可以访问到Dom节点，使用$refs属性对Dom进行操作。
接口请求一般放在mounted中，但需要注意的是服务端渲染时不支持mounted，需要放到created中。

###### updated
发生在更新完成之后，当前阶段组件Dom已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。

###### beforeDestroy
发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

##### computed vs watch
computed本质是一个具备缓存的watcher

##### v-if vs v-show
当条件不成立时，v-if不会渲染DOM元素，v-show操作的是样式(display)，切换当前DOM的显示和隐藏

##### 事件绑定原理
原生事件绑定是通过addEventListener绑定给真实元素的，组件事件绑定是通过Vue自定义的$on实现的。

###### virtual dom
Virtual DOM本质就是用一个原生的JS对象去描述一个DOM节点。是对真实DOM的一层抽象。