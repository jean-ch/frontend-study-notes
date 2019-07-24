##### change without mutation的一点启发
1. change with mutation```arr[1] = 'change'```   
2. without mutation```let temp = arr; temp[1] = 'change'; setData(arr, temp);```
- 方便监听
- 优化reflow
- 用cancat代替push不会对原array产生mutation




- setState
？ 单向绑定和双向绑定    
？ 怎么处理state