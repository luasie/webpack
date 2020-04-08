# webpack
webpack 基本配置

## entry入口
```ｊｓ
entry: {
  index: './src/index.js',
  other: './src/other.js'
}
```

## output出口
```js
output: {
  path: path.resolve(__dirname, 'dist'), // 指定输出文件夹
  filename: 'js/[name].[chunkhash].js',　// chunkhash 可以防止缓存　页面改动时，每次构建的hash都不一样
  hashDigestLength: 8  // 设置hash的长度，也可以通过 filename: [chunkhash:8]　设置
}
```
