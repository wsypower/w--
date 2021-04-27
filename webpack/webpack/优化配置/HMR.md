#  HMR

> Hot module repalcement 热模块替换/模块替换
>
> 作用：一个模块发生变化，只会重新打包一个模块，不会全部重新打包

开启

```js
devServer:{
  hot:true
}
```

对css的模块替换

> 依赖于style-loader、不用管理

```js
// 依赖于style-loader、不用管理
// 这也是为什么在生产环境要用style-loader 在正式环境要提取成单独文件
```

对js文件的处理

> 默认是没有热模块替换功能的,需要修改js代码，添加支持hmr功能的代码       

```js
if(module.hot){
   module.hot.accept('./print.js',function(){
     // 会监听print文件的变化
     print()
   })
}
```

对html

> 默认是没有热模块替换功能的,

```js
entry：['./src/index.js',',/src/index.html']
```

