# eslint语法检查

### 语法检测

> 只针对js语法检查
>
> 只检查自己写的源代码，第三方的库不用检查（exclude:/node_modeles/）

### 推荐使用的规则

> airbnb (github 10w+)
>
> eslint-config-airbnb-base
>
> 为了使用需要三个库 eslint eslint-config-airbnb-base eslint-plugin-import

eslintConfig

```js
"eslintConfig":{
  "extends":"airbnb-base"
}
```



   webpack

```js
module:{
  rules:[
    {
      test:/\.js$/,
      exclude:/mode_modules/,
      loader:'eslint-loader',
      options:{
        fix:true // 自动修复
      }
    }
  ]
}
```

