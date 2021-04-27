# css 提取 

```js
min-css-extract-plugin
```



```javascript
modeule:{
  rules:[
    {
      test:'/\.css$/'
      use:[
      		MinicssExtractPlugin.loader,
      		'css-loader'
      ]
    }
  ]
}
plugins:[
  new MinicssExtractPlugin({
    filename:'css/built.css'
  })
]
```

