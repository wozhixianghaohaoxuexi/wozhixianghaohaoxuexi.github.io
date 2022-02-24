---
title: webpack打包ts
date: 2022-02-24 16:10:27
categories:
  - ts
tags:
---

### 一、基本使用

#### 1. 安装依赖

```powershell
npm i -D webpack webpack-cli typescript ts-loader
```

#### 2. webpack配置

```js
// webpack.config.js

// 引入一个包
const path = require('path');

// webpack 中的所有配置信息都应该写在 module.exports 中
module.exports = {

    // 指定入口文件
    entry: "./src/index.ts",
    
    // 指定打包文件所在的位置
    output: {
        // 打包文件目录
        path: path.resolve(__dirname, 'dist'),
        // 打包文件名
        filename: "bundle.js"
    },
    
    // 指定 webpack 打包时要使用的模块
    module: {
        // 指定要加载的规则
        rules: [
            {
                // 指定规则生效的文件
                test: /\.ts$/,
                // 要使用的 loader
                use: 'ts-loader',
                // 要排除的文件
                exclude: /node-modules/
            }
        ]
    }
}
```

#### 3. ts编译配置

```json
// tsconfig.json
{
  "compilerOptions": {
    "module": "ES2015",
    "target": "ES2015",
    "strict": true
  }
}
```

#### 4. 打包命令配置

```json
// package.json 文件的 scripts 配置中添加 "build": "webpack"，打包时直接执行 npm run build 即可
{
    "scripts": {
        "build": "webpack"
    }
}
```

* 执行 npm run build 打包 ts 文件

### 二、进阶使用

#### 1. html-webpack-plugin

* 作用：自动生成成 html 并引用相关资源（css、js...）
* 安装：`npm i -D html-webpack-plugin`
* 配置：

```js
// webpack.config.js

// 引入 html 插件
const HTMLWebpackPlugin = require('html-webpack-plugin');

module.exports = {
/*
    其他配置参照基本使用，已省略
*/

  // 配置 webpack 插件
  plugins: [
    new HTMLWebpackPlugin({
       // 自定义 html 标题
      title: "自定义title",
      // 指定 html 模板
      template: "./src/index.html"
    })
  ]
}
```

#### 2. webpack-dev-server

* 作用：让项目在 webpack 内置服务器中运行，可根据项目改变自动刷新
* 安装：`npm i -D webpack-dev-server`
* 配置：

```json
// package.json 的 scripts 配置项中添加 start 命令后，通过 npm run start 启动即可

{
    "scripts": {
        "start": "webpack serve --open chrome.exe"
    }
}
```

#### 3. clean-webpack-plugin

* 作用：清除打包后的 dist 目录，保证打包后的文件为最新的。不使用该插件打包文件会进行替换，使用插件是先删除原文件再创建打包文件
* 安装：`npm i -D clean-webpack-plugin`
* 配置：

```js
// webpack.config.js

// 引入clean插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  plugins: [
    new CleanWebpackPlugin()
  ]
}
```

#### 4. 设置引用模块

* 作用：设置 ts 文件可以作为模块使用
* 配置：

```js
// webpack.config.js

module.exports = {
   // 设置引用模块
  resolve: {
    // 表示 ts, js 文件可以作为模块使用，不设置 ts 文件作为模块使用时会报错
    extensions: ['.ts', '.js']
  }
}
```

#### 5. babel 解决兼容性问题

* 作用：将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中
* 安装：`npm i -D @babel/core @babel/preset-env babel-loader core-js`
* 配置：

```js
// webpack.config.js

module.exports = {
  entry: "./src/index.ts",
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: "bundle.js",
    
    // 告诉 webpack 不使用箭头函数。高版本 webpack 打包后的 js 中，包含 webpack 提供的箭头函数（非用户定义），不能兼容 ie
    environment: {
      arrowFunction: false,
      const: false
    }
  },
  module: {
    rules: [{
      test: /\.ts$/,
      // 要使用的 loader，数组从后往前执行
      use: [
        // 配合babel
        {
          // 指定加载器
          loader: "babel-loader",
          // 设置babel
          options: {
            // 设置预定义的环境
            presets: [
              [
                // 指定环境插件
                "@babel/preset-env",
                // 配置信息
                {
                  // 指定兼容的浏览器版本
                  targets: {
                    // 兼容到 chrome 58 版本，和 ie 11 版本
                    "chrome": "58",
                    "ie": "11"
                  },
                  // 指定 corejs 版本
                  "corejs": "3",
                  // 使用 corejs 的方式，"usage"表示按需加载
                  "useBuiltIns": "usage"
                }
              ]
            ]
          }
        },
        'ts-loader'
      ],
      exclude: /node-modules/
    }]
  },
  plugins: [
    new HTMLWebpackPlugin({
      template: "./src/index.html"
    }),
    new CleanWebpackPlugin()
  ],
  resolve: {
    extensions: ['.ts', '.js']
  }
}
```
