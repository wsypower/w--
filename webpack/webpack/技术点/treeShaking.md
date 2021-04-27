# TreeShaking(摇树优化)

## 概念：

> 只把用到的方法打入到bundle，没有用到的方法会再uglify阶段擦除掉

## 适用前提

- 必须使用es6模块化
- 开启production环境

## 缺点

- 容易将引用的css去除掉

改变tree shaking 配置消除此问题

```json
// package.json

// 这样写会吧  css @babel 等干掉
"sideEffects":false // 所有代码都没有副作用（都可以进行tree shaking）

// 修复
"sideEffects":["*.css"] // 所有代码都没有副作用（都可以进行tree shaking）
```



## 使用

> webpack默认支持，在.babelrc 里设置modules:false 即可
>
> production mode 的模式下默认开启
>
> 必须是es6,cjs不支持

原理

DCE(Elimination - 消除 )

- 代码不会被执行，不可到达
- 代码执行的结果
- 代码只会影响死变量（只写不读  ）

利用的是es6模块的特点

- 只能作为模块顶层的语句出现
- import 的模块名只能是字符串常量
- import binding是immutable

代码擦除：uglify阶段擦除无用代码