# Entry

### 单入口打包

> 此时chunk的名称默认是main

```js
entry:'./src/app.js',
output:{
  filename:'[name].js',
  path:resolve(__dirname,'build')
}
```



### 单入口第二种

> 还是只打包一个文件

```js
entry:['./src/react.js'，'./src/react-dom.js'],
output:{
  filename:'[name].js',
  path:resolve(__dirname,'build')
}
```



### 多入口打包

```js
entry:{
  main:'./src/index.js',
  index:"./src/add.js"
}，
output:{
  filename:'[name].js',
  path:resolve(__dirname,'build')
}
```

