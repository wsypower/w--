# PWA 渐进式网络开发应用程序 （离线可访问）

>workbox技术 插件是workbox-webpack-plugin

  serviceworker 必须运行在服务器上

build 要运行在服务器上

````js
plugins:[
  // 生成一个serviceworker文件
  new WordboxWebpackPlugin.GenerateSW({
    // 1.帮助serviceworker快速启动
    // 2.删除旧的serviceworker文件
    clientsClaim:true,
    skipWaiting:true
  })
]
````

```js
//
// 处理兼容性问题serviceworker
if( 'serviceWorker' in navigator.){
    navigator.serviceworker.register('/service-worker').then(res=>{}).catch(err=>{})
 }
```

