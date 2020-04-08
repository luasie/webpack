# webpack
webpack4 基本配置

## 入口　entry
不配置时默认src目录下的index.js文件
```ｊｓ
entry: './src/index.js'
```

## 出口　output
```js
output: {
  path: path.resolve(__dirname, 'dist'), // 指定输出文件夹
  filename: 'js/[name].[chunkhash].js',　//　指定打包的文件名，可以添加输出的路径 chunkhash 可以防止缓存　页面改动时，每次构建的hash都不一样
  hashDigestLength: 8  // 设置hash的长度，也可以通过 filename: [chunkhash:8]　设置
}
```

## 处理html HtmlWebpackPlugin/CleanWebpackPlugin
```js
yarn add html-webpack-plugin  clean-webpack-plugin -D

plugins: [
  new HtmlWebpackPlugin({
    template: './public/index.html'　// 指定模板
    hash: true, // 防治缓存
    minify: {
      removeAttributeQuotes: true  // 压缩　去除引号
    }
  }),
  new CleanWebpackPlugin()  // 每次构建前清空上次的打包内容
],
```

## 处理css及添加前缀 style-loader/css-loader　postcss-loader/autoprefixer
```js
yarn add style-loader css-loader postcss-loader autoprefixer -D

module: {
  rules: [
    {
      test: /\.css$/,
      use: [
        'style-loader'　// 在head生成style节点
        'css-loader', 
        'postcss-loader',
      ] 
    }
  ]
}
```

根目录下`postcss.config.js`文件
```js
module.exports = {
  plugins: [require('autoprefixer')]
}
```

## 提取css文件　MiniCssExtractPlugin
```js
yarn add mini-css-extract-plugin -D

module: {
  rules: [
    {
      test: /\.css$/,
      use: [
        {
          loader: MiniCssExtractPlugin.loader
        },
        'css-loader'
      ] 
    }
  ]
}
plugins: [
  new MiniCssExtractPlugin({
    filename: 'css/[name].[chunkhash].css'  // 指定文件名称及存放路径
  })
]
```

## 处理图片　url-loader
```js
yarn add url-loader -D

rules: [
  {
    test: /\.(png|jpg|gif)$/,
    use: [
      {
        loader: 'url-loader',
        options: {
          limit: 8192,  // 限制多少ｋ内使用base64位，否则使用file-loader产生一张真实的图片
          name: '[name].[hash:8].[ext]',
          outputPath: 'assets', // 文件输出路径
          publicPath: '../assets' // 文件引用路径，使用../asstes是因为上文css文件打包在css文件夹内。不返回上层目录，图片引用地址会在css目录下。
        },
      },
    ]
  },
]
```

## 转译JS文件 babel-loader
```js
yarn add babel-loader @babel/core @babel/preset-env @babel/plugin-proposal-class-properties　@babel/plugin-transform-runtime -D
yarn add @babel/runtime

rules: [
  {
    test: /\.js$/,
    exclude: /node_modules/,  // 排除指定文件夹
    use: [
      {
        loader: 'babel-loader', // 转译es6语法
        options: {
          presets: [
            '@babel/preset-env'
          ],
          plugins: [
            ['@babel/plugin-proposal-class-properties'], // 转译class类
            ['@babel/plugin-transform-runtime']　// 辅助代码从这里引用，可以减少代码体积
          ]
        }
      }
    ]
  }
]
```
babel插件可以通过`.babelrc`文件配置（推荐）
```JSON
{
  "plugins": [
    "@babel/plugin-proposal-class-properties",
    "@babel/plugin-transform-runtime",
  ]
}
```
