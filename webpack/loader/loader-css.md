# Loader解析css

------

[TOC]



## style-loader

> 把 CSS 插入到 DOM 中

```bash
npm install --save-dev style-loader
```



## css-loader

> css-loader` 会对 `@import` 和 `url()` 进行处理，就像 js 解析 `import/require()` 一样。

```bash
npm install --save-dev css-loader
```



## sass-loader

>加载 Sass/SCSS 文件并将他们编译为 CSS。

外部应配置 sass（dart-sass）

```bash
#sass-loader 
#sass dart-sass
npm install sass-loader sass webpack --save-dev
```



## MiniCssExtractPlugin

>本插件会将 CSS 提取到单独的文件中，为每个包含 CSS 的 JS 文件创建一个 CSS 文件，并且支持 CSS 和 SourceMaps 的按需加载。

```bash
npm install --save-dev mini-css-extract-plugin
```

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
    ],
  },
};

```



基本使用

```js
module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          "style-loader",
          {
            loader: "css-loader",
            options: {
              url: true,
            },
          },
        ],
      },
      {
        test: /\.(scss|sass)$/,
        use: [
          miniCssExtractPlugin.loader,
          "css-loader",
          {
            loader: "sass-loader",
          },
        ],
      },
    ],
  },
```

