# dll

> 对某些库单独打包

```js
//webpack.dll.js

const webpack = require('webpack')


modules.exports={
  
  entry:{
		jquery:['jquery'],
  },
  output:{
    filename:'[name].js',
    path:resolve(__dirname,'dll'),
    library:'[name]--[hash]'
  },
  plugins:[
    // 打包一个manifest，提供和jquery映射
    new webpack.DLLPlugin({
      name:'[name]_[hash]',
      path:resolve(__dirname,'dll/manifest.json')
    })
  ]
}
```

```js
//webpack.congfig.js
const addAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin')
 plugins:[
    // 高速webpack的时候那些库不需要打包,同时使用时的名称也要改变
    new webpack.DLLReferencePlugin({
      manmifest:resolve(__dirname,'dll/manifest.json')
    })
   // 讲某个文件打包出去，并在html中引入
   new addAssetHtmlWebpackPlugin( {
   		filepath:resolve(__dirname,'dll/jquery.js')
   })
  ]
```

