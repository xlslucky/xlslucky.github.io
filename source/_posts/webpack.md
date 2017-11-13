---
title: webpack 学习笔记
date: 2017-10-22 20:44:14
tags:
    - webpack
---
> 参考
> ** webpack 2.2中文文档 [http://www.css88.com/doc/webpack2/](http://www.css88.com/doc/webpack2/) **
> webpack 中文指南 [https://webpack.toobug.net/zh-cn/](https://webpack.toobug.net/zh-cn/)
> webpack 从入门到工程实践 [http://dwz.cn/6tw4XA](http://dwz.cn/6tw4XA)
> webpack 优秀文章 [https://github.com/webpack-china/awesome-webpack-cn](https://github.com/webpack-china/awesome-webpack-cn)

# 什么是webpack?
webpack是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。

# webpack和gulp的区别？
webpack是模块化解决方案，gulp是构建工具。


命令行执行webpack配置 [http://www.css88.com/doc/webpack2/api/cli/](http://www.css88.com/doc/webpack2/api/cli/)
```shell
# progress 编译进度百分比
# watch 监控
$ webpack --progress --watch
```

# 配置

先来一段示例
```js
var webpack = require('webpack');
var path = require('path');

module.exports = {
  devtool: 'source-map',
  entry: './src',
  output: {
    path: __dirname + '/dist/',
    filename: '[name].js'
  },
  module: {
    /*loaders: [
      {
        test: /\.jsx?$/,
        loader: 'babel-loader'
      }
    ]*/
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: ['babel-loader'],
        /*use: {
          loader: "babel-loader",
          options: {
            presets: ["es2015", "react"]
          }
        }*/
      },{
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },
  externals: {
    // 声明一个外部依赖
    'react': 'window.React',
    'react-dom': 'window.ReactDOM',
    'react-router': 'window.ReactRouter',
  },
  resolve: {
  // 文件扩展名，写明以后就不需要每个文件写后缀
    extensions: ['.js', '.css', '.json'],
  // 路径别名，比如这里可以使用 css 指向 static/css 路径
    alias: {
      '@': __dirname + '/src',
      'css': __dirname + '/assets/css'
    }
  },
  plugins: [
    // 压缩js代码 加入了这个插件之后，编译的速度会明显变慢，所以一般只在生产环境启用。
    // warnings: false默认false
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    }),
    // 把代码里所有的 process.env.NODE_ENV 替换为 test
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('test')
    }),
    // 跳过编译时出错的代码并记录，使编译后运行时的包不会发生错误
    new webpack.NoErrorsPlugin(),
    // 提取公共模块
    new webpack.optimize.CommonsChunkPlugin('common')
  ]
}
```

## 入口(entry)

webpack.config.js
```js
// 简写
module.exports = {
  entry: './src/index.js'
}
// 等于
module.exports = {
  entry: {
    main: './src/index.js'
  }
}
```

## 输出(output)

1. output.filename 打包之后的文件名 推荐 `main.js`、`bundle.js`、`index.js`
  * [name]              *（文件名）*
  * [hash]              *（多个文件打包，每次发生改变都会生成同一个哈希码）*
  * [chunkhash]         *（多个文件打包，只有改变的那个文件才会再生成哈希码）*

2. output.path 打包之后存放的路径，为 `绝对路径`
3. output.publicPath **搞不懂**
4. output.chunkFilename **搞不懂**

webpack.config.js
```js
module.exports = {
  entry: {
    one: './app/one.js',
    two: './app/two.js'
  },
  output: {
    path: __dirname + '/public',
    // [hash] 多个文件哈希码相同，改变单独文件，所有的哈希码都会同时改变
    // filename: '[name].[hash].js'    // one.a3c0e8239ba6e5f788c7.js && two.a3c0e8239ba6e5f788c7.js
    // [chunkhash] 每个文件哈希码都不同，改变文件只会改变当前文件的哈希码
    filename: '[name].[chunkhash].js'  // one.0e180c77412ff8083e5a.js && two.5b800ad2b18951ff60fa.js
  }
}
```

## 加载器(loaders)
> loader对应用程序中资源文件进行转换。

**安装**
```shell
$ npm install css-loader --save-dev    # 加载css文件
$ npm install ts-loader --save-dev     # TypeScript转化为JavaScript
```

webpack.config.js
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {loader: 'style-loader'},
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      },
      {
        test: /\.ts$/,
        use: ['ts-loader']
      }
    ]
  }
}
```

## 插件(plugins)
> 插件目的在于解决loader无法解决的事。

webpack.config.js
```js
var webpack = require('webpack');           // 访问内置插件
var path = require('path');

module,exports = {
  entry: './src',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        use: ['babel-loader']
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin()
  ]
}
```