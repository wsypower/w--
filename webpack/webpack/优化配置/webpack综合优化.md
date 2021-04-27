# Webpack性能优化

## 开发环境

- 优化构建速度
  - HMR
- 优化调试
  - source-map

## 生产环境

- 优化打包速度
  - one-of  一下loader 只会匹配一个（注意：不能有两项配置处理一种类型的文件）
  - babel缓存
  - 多进程打包
  - externals(让那些苦不打包，直接用CDN引用)
  - dll（让默些库不打包，单独打包，html静态引入）
- 优化代码运行效率
  - 缓存（has contenthas chunkhash）
  - Three shaking
  - Code split
    - Import 懒加载
    - optimization 代码分割
  - 懒加载/预加载
  - PWA
  - 多进程压缩

