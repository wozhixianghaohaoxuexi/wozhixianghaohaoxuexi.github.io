---
title: webpack性能优化
date: 2022-01-06 11:44:25
categories: 打包工具
tags:
- webpack
---

### 一、开发环境性能优化

#### 1、优化打包构建速度

##### 1.1、HMR

HMR：hot module replacement 热模块替换 / 模块热替换。

* 作用：一个模块发生变化，只会重新打包这一模块，而不是重新打包所有模块，极大提升构建速度。
    * 样式文件：可以使用 HMR 功能，因为style-loader内部实现了。
    * js文件：默认不能使用 HMR 功能 -> 需要修改js代码，添加支持 HMR 功能的代码。
        * 注意：HMR 功能对 js 的处理，只能处理非入口 js 文件的其他文件。
    * html文件：默认不能使用 HMR 功能，同时会导致问题： html文件不能热更新了（修改后不会重新编译，刷新浏览器）。
        * 解决：修改entry入口，将html文件引入。
        * 由于html文件只有一个，因此不用做 HMR 功能。

##### 1.2、source-map

source-map：一种提供源代码到构建后代码映射的技术（如果构建后代码出错了，通过映射可以追踪源代码错误）
```js
// 直接在 webpack.config.js 中添加 devtool 即可开启source-map
devtool: 'source-map'
```
devtool 取值为 `[ inline- | hidden- | eval- ] [ nosources- ] [ cheap- [ module- ] ] source-map`

* * *

* 内联和外部的区别：
    * 1、外部生成了文件，内联没有
    * 2、内联构建速度更快    

* * *

* source-map：外部
    * 错误代码准确信息 和 源代码的错误位置
* inline-source-map：内联
    * 只生成一个内联source-map
    * 错误代码准确信息 和 源代码的错误位置
* hidden-source-map：外部
    * 错误代码的错误原因，但是没有源代码错误位置，只能提示到构建后代码的错误位置
* eval-source-map：内联
    * 每一个文件都生成对应的source-map，都在eval
    * 错误代码准确信息 和 源代码的错误位置
* nosources-source-map：外部
    * 错误代码的错误信息，但没有任何源代码信息
* cheap-source-map：外部
    * 错误代码准确信息 和 源代码的错误位置
    * 只能精确到行（当错误代码与正确代码在同一行时，整行标志整行，包含正确代码）
* cheap-module-source-map：外部
    * 错误代码准确信息 和 源代码的错误位置
    * module会将 loader 的 source map 加入

* * *

* 开发环境：要求速度快，调试更友好
    * 速度快（eval > inline > cheap > ...）
        * eval-cheap-source-map
        * eval-source-map
    * 调试更友好
        * source-map
        * cheap-module-source-map
        * cheap-source-map
    * --> eval-source-map / eval-cheap-module-source-map
* 生产环境：源代码是否隐藏？调试是否要友好？
    * 内联会让代码体积变大，所以生产环境不用内联
    * nosources-source-map 源代码和构建后代码全部隐藏
    * hidden-source-map 只隐藏源代码，会提示构建后代码错误信息
    * --> source-map / cheap-module-source-map

##### 1.3、oneOf

##### 1.4、缓存

* babel缓存（让第二次打包构建速度更快）
    * cacheDirectory: true
* 文件资源缓存（让代码上线运行缓存，加快相应速度）
    * hash：每次 webpack 构建时会生成一个唯一的 hash 值。
        * 问题：因为 js 和 css 同时使用一个 hash 值，如果重新打包，会导致所有缓存失效。（可能只改动一个文件）
    * chunkhash：根据 chunk 生成的 hash 值。如果打包来源于同一个 chunk，那么 hash 值就一样。
        * 问题：js 和 css 的 hash 值还是一样的，因为 css 是在 js 中被引入的，所以同属于一个chunk
    * contenthash：根据文件的内容生成 hash 值，不同文件 hash 值一定不一样 

##### 1.5、tree shaking

tree shaking：去除应用程序中没有使用的代码，让代码体积更小

* 前提：1、必须使用 ES6 模块化    2、开启 production 环境
* 作用：减少代码体积

* * *

* 可能由于 webpack 版本问题，将 css 等文件去除，在 package.json 中配置 "sideEffects": false 模拟：
    * "sideEffects": false 所有代码都没有副作用（都可以进行tree shaking）
    * 问题：可能会把 css / @babel/polyfill 等（副作用）文件去除 
    * 解决："sideEffects": ["\*.css", "\*.less"]，最好添加该配置，否则可能因为 webpack 版本问题去除 css 等文件

##### 1.6、code split

code split：将打包生成的一个文件分割为多个文件

* 方案一：单入口 / 多入口文件，每个入口打包成一个文件
* 方案二：webpack.config.js 配置中添加 optimization（一般都是单入口文件，该方案使用较少）

```js
// 1、可以将node_modules中代码单独打包一个chunk最终输出
// 2、自动分析多入口chunk中，有没有公共的文件。如果有，会打包成一个单独chunk
optimization: {
    splitChunks: {
        chunks: 'all'
    }
}
```

* 方案三：单入口，配置中添加 optimization，并通过js代码，让某个文件被单独打包成一个chunk
 
```js
// import动态导入语法，能让某个文件单独打包
import(/* webpackChunkName: 'test' */'.test')
    .then(({ mul, count}) => {
        // 文件加载成功
        console.log(mul(2, 5));
    })
    .catch(() => {
        // 文件加载失败
    });
```

##### 1.7、懒加载和预加载

* 懒加载：当文件需要使用时才加载
* 预加载 prefetch（会有兼容性问题）：会在使用之前，提前加载js文件
* 正常加载可以认为是并行加载（同一时间加载多个文件），而预加载是等其他资源加载完毕，浏览器空闲了，再偷偷加载资源

```js
document.getElementById('btn').onclick = function() {
    // 点击时，动态引入为懒加载，设置 webpackPrefetch: true 为预加载
    import(/* webpackChunkName: 'test', webpackPrefetch: true */'./test')
        .then(({ mul }) => {
            console.log(mul(2, 5));
        });
};
```

##### 1.8、PWA

PWA：渐进式网络开发应用程序（离线可访问）。webpack中使用workbox-webpack-plugin插件

```js
// 在webpack中使用workbox-webpack-plugin插件
const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

module.exports = {
    plugins: [
        new WorkboxWebpackPlugin.GenerateSW({
            /*
                1、帮助 serviceworker 快速启动
                2、删除旧的 serviceworker
                
                打包时会自动生成一个 serviceworker 配置文件
            */
            clientsClain: true,
            skipWaiting: true
        })
    ]
}

// 在入口文件中注册 serviceworker
// 注册 serviceworker，并处理兼容性问题
/*
    注意： 
    1、eslint不认识 window、navigator 全局变量
        解决：需要修改 package.json 中 eslintConfig 配置
        "env": {
            // 支持浏览器端全局变量
            "browser": true
        }
    2、sw代码必须运行在服务器上
        --> 快速创建服务器库 serve
            npm i serve -g
            serve -s build 启动服务器，将build目录下所有资源作为静态资源暴露出去    
*/
if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker
            .register('/service-worker.js')
            .then(() => {
                console.log('sw注册成功了');
            })
            .catch(() => {
                console.log('sw注册失败了');
            });
    });
}
```

##### 1.9、多进程打包

使用 thread-loader 插件

##### 1,10、externals

##### 1.11、dll

#### 2、优化代码调试

### 二、生产环境性能优化

#### 1、优化打包构建速度

#### 2、优化代码运行的性能