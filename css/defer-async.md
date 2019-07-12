defer和async都是script标签的属性   
延迟执行，不阻塞当前文档的解析  

#### defer
开启新线程，不阻塞  
- 只适用外联脚本，没有src属性的script不要用defer  
- 多个defer的script会按顺序执行
- defer在DOMContentLoaded和Load之间执行

#### async
异步下载脚本，会在脚本下载完成后立即执行 
- 
- 有依赖关系的脚本不宜用async
- 只适用外联脚本
- 多个async，都是异步的，但不能保证执行顺序
- 在Load之前执行，不确定和DOMContentLoaded的顺序关系

#### DOMContentLoaded和Load的区别
- DOMContentLoaded
  - HTML文档解析完成，不需要等待图片等其他资源下载完成
- Load
  - 所有资源下载完成
