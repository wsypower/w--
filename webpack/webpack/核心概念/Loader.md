# Module

```js
module:{
  rules:[
    // loader的配置
    {
      test:/\.css$/,
      // 多个loader
      use:[
        'style-loader',
        'css-loader'
      ]
    },
    // 单个loader
    {
      test:/\.js$/,
      loader:'eslint-loader',
      // 排除
      exclude:/node_modules/，
      // 只检查
      include:path.resloce(__dirname,'src'),
    	// 优先执行
    	enforce:'pre'，
    	// 延后执行
    	enforce:'post'，
    	// 以下loader 只生效一个
    	oneOf:{}
    }
  ]
}
```

