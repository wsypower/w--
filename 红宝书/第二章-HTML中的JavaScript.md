# 第二章  HTML中的JavaScript

## script元素

### 元素属性

- async - 立即下载,立即执行,非顺序执行
- charset - src属性指定的代码字符集
- crossorigin  - CORS跨资源共享
- defer - 立即下载,延迟执行,顺序执行
- intergrity - 验证完整性
- language -废弃
- src 
- type 
  - module  
    -  代表es6模块  
    - 立即下载,延迟执行,顺序执行，添加async 就具有async特性

![截屏2021-02-20 上午8.30.55](/Users/weiyafei/Desktop/截屏2021-02-20 上午8.30.55.png)

## <noscript>元素

> 可以包含任何可以出现在body中的HTML元素（script除外）

  触发条件

1. 浏览器不支持脚本
2. 浏览器对脚本的支持被关闭

