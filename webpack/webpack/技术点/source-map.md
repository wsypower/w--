# source-map

- source-map   精确到行列，会有source-map文件
- cheap    精确到行，不提示列，不管loader
- module   提示loader问题
-   inline  没有map文件， 精确到行列
- eval    通过eval 提示，速度快

## 最佳实践

开发环境

```js
cheap-module-eval-source-map // 提示比较全，速度相对较快
```

正式环境

```js
cheap-module-source-map // 提示效果会更好
```

经常问source-map原理