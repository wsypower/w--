# resolve

> 解析模块的规则

```js
resolve:{
  // 解析配置别名
  alias:{
    // 可以简写路径  
    $css:path.reolve(__dirname,'src/css')
  }，
  // 省略文件滤镜后缀名    
  extensions:['.js','.css']，
  // 告诉webpack解析模块是去那个目录找
  modules:[path.reolve(__dirname,'node_modules')，'node_modules']
}
```

