defer和async都是script标签的属性   
开启新线程，延迟执行，不阻塞当前文档的解析  

#### defer
异步下载脚本，但下载完成后不立即执行脚本，而是在load之后DOMContentLoaded之前执行    
- 只适用外联脚本，没有src属性的script不要用defer  
- 多个defer的script会按顺序执行
- defer在DOMContentLoaded和Load之间执行

#### async
异步下载脚本，会在脚本下载完成后立即执行 
- 有依赖关系的脚本不宜用async
- 只适用外联脚本
- 多个async，都是异步的，但不能保证执行顺序
- 在Load之前执行，不确定和DOMContentLoaded的顺序关系

#### DOMContentLoaded和Load的区别
- DOMContentLoaded
  - 当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发  
  - 无需等待样式表、图像和子框架的完成加载
- Load
  - 用于检测一个完全加载的页面    
  - 当一个资源及其依赖资源已完成加载时，将触发load事件
