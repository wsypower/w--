# optimization

> 必须要在生产环境才有意思

```js
optimization:{
  splitChunks:{
    	chunks:'all',
      minSize:30*1024, //最小30kb
      maxSize:0 // 最大没限制，多大都分割
    	minChunks:1 // 要提取的chunks 最少被引用一次
    	maxAsyncRequests:5 // 按需加载，并行加载的文件最大数量文件
    	maxInitialRequests:3 // 入口最大并行请求数量3
    	automaticNameDelimiter:'~' // 名称连接符
    	name:true // 可以使用命名规则
    	cacheGroups:{ // 分割的chunk组
        vendors:{
          test:/[\\/]node_modules[\\]/ 
          // 优先级为-10
          priority:-10，
        }，
        // 默认组
        default:{
          minChunks:2,
          priority:-20，
          // 若当前要打包的模块，和之前已经被提取的模块是同一个，就会被复用
          reuseExistingChunk:true
        }
      }，
      // 
      runtimeChunk:{
        // 将当前模块的记录其他模块的hash单独打包为一个runtime文件
        name:entrypoint=>`runtime-${entrypoint.name}.js`
      }，
      minimizer:{
          // 配置生产环境的压缩 
        new TerserWebpackPlugin({
          // 开启缓存
          cache:true,
         	sourmap:true 
        })
      }
  }
}
```

