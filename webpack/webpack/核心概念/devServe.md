# DevServe

```js
devServe:{
    contentBase:path.resolve(__dirname,'build'),
    // 监视contentBase 目录下的所有文件，一旦文件有变化就会resolve 
    watchContentBase:true
  	watchOptions:{
      // 忽略文件
      ignored:/node_modules/,
    }
  	// 启动gzip压缩
    compress:true
  	port:8080,
    open:true,
    host:'localhost',
    // 不需要启动服务器的日志
    clientLogLevel:'none'
  	// 除了一些基本的启动信息以外，其他内容不要显示
    quiet:true，
    // 出错了不要全屏提示
    overlay:false，
    // 服务器代理
    proxy:{
      // 做了一个服务器请求转发
      'api':{
						target:'http://locallhost:8080'，
            // 路径重写， 去掉api
            pathRewrite:{
              '^/api':''
            }
      }
    }
  	
    
}
```

