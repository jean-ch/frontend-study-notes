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