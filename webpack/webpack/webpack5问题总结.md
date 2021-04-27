## webpack5问题总结

### 1.webpack-dev-seve 热更新失效

```js
// 添加
// 文档说明是‘web’默认值，但是不管怎么更改版本号，无效，必须写这句，就很无语哦
target: "web",
```

