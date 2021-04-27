# Output

```js
output:{
  filename:'js/[name].js',
  path:path.resolve(__dirname,'dist'),
  //生产环境，所有输出资源的引用的公共路径的前缀
  publicPath:'/'，
  //非入口chunk的名称
  chunkFilename:'[name]_chunk.js' 
  //整个库向外暴露的变量名
  library:'[name]'，
  //添加到那个brower上
  libraryTarget:'window' // 可指定commonjs amd umd
}
```

chunkFilename 产生的由来

- import懒加载  
- Optimazations

