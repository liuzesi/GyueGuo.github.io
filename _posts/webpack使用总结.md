---
layout: post
title: webapck使用总结（未完成）
date: 2017-11-13
categories: blog
tags: [webapck]
description: webapck使用总结
---

# webapck 使用总结

webapck是一个模块打包工具，它将javascript模块、less、sass编译成浏览器可用资源。
github项目链接 [https://github.com/webpack/webpack](https://github.com/webpack/webpack)

### 为什么要使用webpack

在开发中，我们经常需要抽取公共模块以供其他程序使用，但是当模块渐多后，依赖关系比较难以管理。而传统工作模式是将公共模块抽取到一个单独的js文件中，使用的时候通过script标签引入，使用commonjs、requirejs、seajs之类的工具解决依赖。然而这样会导致页面加载时浪费很多http请求。
而webpack帮我们解决了这个问题，webpack可以将引入的模块抽取出公用模块进行打包。而且webpack还有很多loader（插件或者叫中间件），通过这些loader，我们可以完成es6转es5、语法检查、编译less等操作。webpack大大简化了开发复杂度，可以更好的进行模块式开发。

### 使用方法及配置步骤（已安装webpack、npm）
![项目目录]
1、安装webpack
```javascript
  npm install wepback (--save-dev) | (-g)
```

2、编写配置文件
文件命名为 webpack.config.js 放在项目根目录下。
```javascript
  const path = require('path')
  const webpack = require('webpack')
  const HtmlWebpackPlugin = require('html-webpack-plugin')
  const ExtractTextPlugin = require('extract-text-webpack-plugin')
  const CleanWebpackPlugin = require('clean-webpack-plugin')
  const FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')

  const entires = {
    index: __dirname + '/app/resource/js/index.js'
  };

  let plugins = [
    new CleanWebpackPlugin(['build']),
    new webpack.optimize.UglifyJsPlugin(),
    new ExtractTextPlugin('style.css'),
    new FriendlyErrorsPlugin()
  ];
  Object.keys(entires).forEach(function(item) {
    console.log(path.resolve(__dirname, './build/views/' + item + '.html'));
    plugins.unshift(new HtmlWebpackPlugin({ 
      filename:  path.resolve(__dirname, './build/views/' + item + '.html'),
      template: path.resolve(__dirname, './app/views/' + item + '.html'),
      inject: true,
    }));
  });

  module.exports = {
    entry: entires,
    output: {
      path: __dirname + '/build', //打包后的文件存放的地方
      publicPath: '/',
      filename: './resource/js/[name]-[hash].js' //打包后输出文件的文件名
    },
    module: {
      rules: [{
          test: /\.(js|vue)$/,
          enforce: 'pre', // 在babel-loader对源码进行编译前进行lint的检查
          include: /src/, // src文件夹下的文件需要被lint
          use: [{
            loader: 'eslint-loader',
            options: {
              formatter: require('eslint-friendly-formatter') // 编译后错误报告格式
            }
          }] // exclude: /node_modules/ 可以不用定义这个字段的属性值，eslint会自动忽略node_modules和bower_
        },
        {
          test: /\.js$/,
          loader: 'babel-loader',
          include: [path.resolve(__dirname + '/app')]
        },
        {
          test: /\.css$/,
          use: ExtractTextPlugin.extract({
            fallback: 'style-loader',
            use: [{
              loader: 'css-loader',
              options: {
                modules: true
              }
            }, {
              loader: 'postcss-loader'
            }],
          })
        },
        {
          test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
          loader: 'url-loader',
          options: {
            limit: 10000,
            name: './images/[name].[hash:7].[ext]'
          }
        },
        {
          test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
          loader: 'url-loader',
          options: {
            limit: 10000,
            name: './fonts/[name].[hash:7].[ext]'
          }
        }
      ]
    },
    resolve: {
      extensions: ['.js', '.css', '.vue'] // require时省略的扩展名，如：require('module') 不需要module.js 
    },
    devtool: '#source-map',
    devServer: {
      contentBase: './build', //本地服务器所加载的页面所在的目录
      hot: true,
      historyApiFallback: true, //不跳转
      inline: true, //实时刷新
      port: 9089
    },
    plugins: plugins
  }

```