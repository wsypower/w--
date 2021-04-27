# babel

## 安装基本配置

只能转换基本语法，如Promise就不能再转换了

```js
babel-loader @babel/core @babel/preset-env
```



## 兼容性处理

### 1. 全部js的处理,引入即可

问题是我只要解决部分兼容性问题，但是被全部引入

```js
@babel/polyfill
```

 

### 2.按需加载 ---> core-js

```js
presets:[
  [
    '@babel/preset-env',
    {
      // 按需加载
      useBuiltIns:'usage',
      corejs:{
        version:3
      }
    }
  ]
]
```

