# PostCSS

> PostCSS是一个用JS插件转换CSS的工具。这些插件可以支持变量和mixins，转换未来的CSS语法，内联图像等等。

## Webpack

```js
postcss ---> postcss-loader  --->postcss-preset-env
```



## 常用插件

- #### [Autoprefixer](https://github.com/postcss/autoprefixer)

- #### [PostCSS Assets](https://github.com/assetsjs/postcss-assets)

- #### [cssnext](http://cssnext.io/)

- #### [Rucksack](http://simplaio.github.io/rucksack/)

- #### [Stylelint](https://github.com/stylelint/stylelint)

- #### [CSS MQPacker](https://github.com/hail2u/node-css-mqpacker)

- #### [cssnano](http://cssnano.co/)

  

  ### Autoprefixer

  ```bash
  npm install --save-dev autoprefixer
  ```

  随着浏览器对规范的支持越来越好，供应商选择替代方案如基于标记的属性使能，特定浏览器前缀如 `-webkit-`、 `-moz-` 以及 `-ms-` 正逐渐消亡。不幸的是，你还无法避免使用供应商前缀，但你很难知道哪些前缀是一直需要的（例如 [user-select](http://caniuse.com/#feat=user-select-none)），哪些是偶尔需要的（例如 [3D 转换](http://caniuse.com/#feat=transforms3d)），哪些是从不需要的（例如 [border-radius](http://caniuse.com/#feat=border-radius)）

  

  ### PostCSS Assets

  PostCSS Assets 提供了很多实用的图像处理函数：

  ```bash
  npm install --save-dev postcss-assets
  ```

  1. **归约 URL：** 通过给定文件名，PostCSS Assets 会使用根路径或完全合法的 URL 来替换 `resolve(image)`。
  2. **处理尺寸：** PostCSS Assets 会使用一个等价的像素值来替换 `width(image)`， `height(image)` 或 `size(image)`。
  3. **内联图像：** PostCSS Assets 会使用 Base64 编码的字符串替换 `inline(image)`。
  4. **清除缓存：** PostCSS Assets 会给图像引用添加一个随机的查询字符串来确认加载的是最新的文件。

  

  ### cssnext 

  cssnext 让你现在就能使用最新的 CSS 语法。

  ```bash
  npm install --save-dev postcss-cssnext
  ```

  

  ### Rucksack

  Rucksack 提供了[很多函数](http://simplaio.github.io/rucksack/docs/)，它的开发者宣称，它们会使 CSS 开发再次有趣起来。

  ```bash
  npm install --save-dev rucksack-css
  ```

  

  ### Stylelint

  Stylelint 基于 140 个规则报错，这些规则被设计用来捕获常见错误，实现样式协议并强制最佳实践。有很多选项可用来依你的喜好配置 Stylelint  —— Pavels Jelisejevs 的文章[使用 PostCSS 提升你的 CSS 质量](https://www.sitepoint.com/improving-the-quality-of-your-css-with-postcss/)带你走过搭建过程。

  

  ### cssnano

  cssnano 会压缩你的 CSS 文件来确保在开发环境中下载量尽可能的小。通过下面的命令安装它：

  ```bash
  npm install --save-dev cssnano
  ```

  

