---
title: webpack4 文档
date: 2018-11-14 15:47:27
tags: JS
category:
- JS
---
> webpack是一个现代Javascript应用程序的静态模块打包器(module bundler)。当webpack处理应用程序时，它会构建一个依赖关系图，其中包含应用程序需要的每个模块，然后将这些模块打包成一个或多个bundle。

<br/>

### 1. 概念
#### 1.1 entry
入口指示webpack应该使用哪个模块，来作为构建其内部依赖图的开始。
向`entry`属性传入数组，将创建多个主入口。常见场景:
- 分离 应用程序app 和第三方库vendor 入口
```
entry: {
  app: './src/app.js',
  vendors: './src/vendors.js',
}
```
- 多页面应用程序
```
entry: {
  pageOne: './src/pageOne/index.js',
  pageTwo: './src/pageTwo/index.js'
}
```

#### 1.2 output
output告诉webpack在哪里输出它所创建的bundles, 以及如何命名这些文件，默认值为`./dist`。
output属性的最低要求包含这两点，`filename`输出文件的文件名，`path`目标输出目录的绝对路径。
- 将一个单独的 bundle.js 文件输出到 /dist 目录中
```
output: {
  filename: 'bundle.js',
  path: '/dist'
}
```
- 多个入口起点
```
output: {
  filename: '[name].js',
  path: __dirname + '/dist',
}
```
- 使用CDN和资源hash的复杂示例


- *loader*
  loader让webpack能够去处理那些非Javascript文件(Webpack自身只理解Javascript)。loader可以将所有类型的文件转换为webpack能够处理的有效模块，然后你就可以利用webpack的打包能力，对它们进行处理。
  `test`用于标识出应该被对应loader进行转换的文件后缀。
  `use`表示进行转换时，应该使用哪个loader。

- *plugins*
  插件可用于执行范围更广的任务。从打包优化和压缩，一直到重新定义环境中的变量。
  要使用一个插件，你需要`require()`它，添加到`plugins`数组中。当需要多次使用同一个插件，需要通过`new`操作来创建它的一个实例。

- *mode*
  development或production

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin'); // npm安装

const config = {
  entry: './path/entry.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({ template: './src/index.html '})
  ]
}

module.exports = config;
```
<br/>


### 3. output
<br/>

### 4. loader
loader支持链式传递。loader链中的一个loader返回值给下一个loader，在最后一个loader，返回webpack所预期的javscript。有三种使用loader的方式，首先安装相应的loader`npm install --save-dev css-loader`
- *配置*
```
module: {
  rules: [
    test: /\.css$/,
    use: [
      { loader: 'style-loader' },
      { loader: 'css-loader', options: {modules: true} },
    ]
  ]
}
```
- *内联*
```
import Style form 'style-loader!css-loader?modules!./style.css'
```
- *CLI*: 在shell命令中指定它们, 这会对.jade文件使用jade-loader,对.css文件使用style-loader和css-loader
```
webpack --module-bind jade-loader -module-bind 'css=style-loader!css-loader'
```
如何编写loader？

<br/>

### 5. webpack模块
webpack模块能够以各种方式表达它们的依赖关系。如
- ES2015 `import`语句
- CommonJS `require()`语句
- AMD `define`和`require`语句
- css/sass/less文件中的`@import`语句
- 样式`url(...)`或HTML中`<img src="...">`中的图片链接