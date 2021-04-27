# source-map

> !webpack5发生重大变化
>
> 提供源代码映射到构建代码的一种技术

## webpack4开启

>Inline | hidden | eval | nosource | cheap | module | source-map

```js
devtool:'source-map'
```

```js
// Inline-source-map  内联  只生成一个source-mao
// hideen-source-map  外部
// eval-source-map    内联  每一个文件都声称一个eval内的source-map
// nosource-source-map  外部
// cheap-source-map  外部
// cheap-module-source-map 外部
```

| key名      | 含义                                                         |
| :--------- | ------------------------------------------------------------ |
| cheap      | 能提示错误代码的准确信息，和源代码的错误位置，精确到行,不管loader |
| Inline     | 能提示错误代码的准确信息，和源代码的错误位置，精确到行列，内联map(其次最快) |
| eval       | 能提示错误代码的准确信息，和源代码的错误位置，精确到行列,用eval内联提示,(最快的) |
| nosources  | 能找到错误代码的准确信息，但是没有任何源代码信息（隐藏源代码） |
| module     | 提示loader问题                                               |
| source-map | 能提示错误代码的准确信息，和源代码的错误位置，精确到行列，有map文件 |
| hideen     | 提示到错误代码的错误原因，但是没有错误位置，不能追踪到源代码的错误，只能提示构建后代码（隐藏源代码） |

#### 调试更友好

```js
cheap-module-source-map
```

#### 速度快

```js
eval-cheap-source-map
eval-source-map
```

#### 开发环境

```js
eval-cheap-module-source-map // 提示比较全，速度相对较快 
eval-source-map //提示最全，相对较快 （vue,react）
```

#### 生产环境

```js
cheap-module-source-map // 提示效果会更好
source-map 
```

