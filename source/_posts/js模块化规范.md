---
title: js模块化规范
date: 2022-01-06 11:31:17
categories: js
tags: 
- js
- 模块化
---

### 一、入门介绍

#### 1、什么是模块/模块化

* 将一个复杂的程序依据一定的规则（规范）封装成几个块（文件），并进行组合在一起
* 块的内部数据/实现是私有的，只是向外部暴露一些接口（方法）与其他外部模块通信

#### 2、为什么要模块化

* 降低复杂度
* 提高解耦性（降低耦合度）
* 部署方便（功能点明确）

#### 3、模块化的好处

* 避免命名冲突（减少命名空间污染）
* 更好的分离，按需加载
* 更高复用性
* 高可维护性

#### 4、页面引入加载script问题

* 请求过多
* 依赖模糊
* 难以维护

### 二、模块化规范

#### 0、不使用任何规范

```javascript
// module1.js
// 定义一个没有任何依赖的模块
(function (window) {
  let name = 'module1'
  function getName() {
    return name
  }
  window.module1 = {getName}
})(window)

// module2.js
// 定义一个有依赖的模块
(function (window, module1) {
  let msg = 'module2'
  function foo() {
    console.log(msg, module1.getName())
  }
  window.module2 = {foo}
})(window, module1)

// app.js
(function (module2) {
  module2.foo()
})(module2)

// html中引入
<!-- 需要发三次请求，且依赖关系不能乱 -->
<script src="./js/module1.js"></script>
<script src="./js/module2.js"></script>
<script src="./app.js"></script>
```

#### 1、CommonJS（掌握）

##### 1.1、规范

* 说明：
    * 每个js文件都可当做一个模块
    * 在服务器端：模块的加载是运行时同步加载的
    * 在浏览器端：模块需要提前编译打包处理
* 基本语法：
    * 暴露模块：
        * `module.exports = value`
        * `exports.xxx = value`
        * 暴露模块本质暴露的是exports对象
    * 引入模块：`require(xxx)`
        * 第三方模块：xxx为模块名
        * 自定义模块：xxx为模块文件路径

##### 1.2、实现

* 服务端实现：Node. js

```javascript
// module1.js
// module.exports = value 暴露一个对象
module.exports = {
  msg: 'module1',
  foo () {
    console.log(this.msg)
  }
}

// module2.js
// 暴露一个函数 module.exports = function(){}
module.exports = function () {
  console.log('module2')
}

// module3.js
// exports.xxx = value
exports.foo = function () {
  console.log('foo() module3')
}
exports.bar= function () {
  console.log('bar() module3')
}

// 将其他模块汇集到主模块
let uniq = require('uniq') // 引入第三方不用写路径，一般第三方放到最上面
let module1 = require('./modules/module1')
let module2 = require('./modules/module2')
let module3 = require('./modules/module3')

module1.foo()
module2()
module3.foo()
module3.bar()
```

* 浏览器端实现：Browserify，也称为浏览器端的打包工具（ES6也使用到）。全局安装`npm install browserify -g`，项目中安装`npm install browserify --save-dev`
    * 浏览器端与node端基本相似，区别在于浏览器端需要将主模块js文件打包，并在html文件中引入打包后的js文件
    * 踩坑：使用`browserify js/src/app.js -o js/dist/bundle.js`对js主模块进行打包时，会报错`browserify : 无法加载文件 D:\devSoftware\nodejs\node_global\browserify.ps1，因为在此系统上禁止运行脚本`，解决方法：以管理员身份进入命令行，切换到项目根目录，再执行上述打包命令

#### 2、AMD（掌握）

##### 2.1、规范

* 说明：专门用于浏览器端，模块的加载是异步的
* 基本语法：
    * 定义暴露模块
        * 定义没有依赖的模块：`define(function() { return 模块 })`
        * 定义有依赖的模块：`define(['module1', 'module2'], function(m1, m2) { return 模块 })`
    * 引入使用模块（可以使用require，也可以使用requirejs）
        * `requirejs(['module1', 'module2'], function(m1, m2) { 使用m1/m2 })`

##### 2.2、实现（浏览器端）

* require.js，在官网或github下载require.js，并将其导入项目（如：js/libs/require.js）

```javascript
// dataService.js
// 定义没有依赖的模块
define(function () {
  let name = 'dataService.js'
  function getName () {
    return name
  }
  // 暴露模块
  return {getName}
})

// alerter.js
// 定义有依赖的模块
define(['dataService', 'jquery'], function (dataService, $) {
  let msg = 'alerter.js'
  function showMsg() {
    console.log(msg, dataService.getName())
  }
  $('body').css('background', 'pink')
  // 暴露模块
  return {showMsg}
})

// main.js
(function () {
  requirejs.config({
    // 基本路径，使用baseUrl出发点在根目录下，不使用从自身出发
    // baseUrl: 'js/',
    // 配置路径
    paths: {
      // 模块名: 路径，文件名后不用加.js，requirejs会自动添加.js
      dataService: './modules/dataService',
      alerter: './modules/alerter',
      // jQuery官方在AMD规范中暴露jquery
      jquery: './libs/jquery-3.6.0'
    }
  })
  requirejs(['alerter'], function(alerter) {
    alerter.showMsg()
  })
})()

// html中引入
<!-- data-main为根目录下主模块路径，src为根目录下require.js路径 -->
<script data-main="js/main.js" src="js/libs/require.js"></script>
```

#### 3、CMD（了解）

##### 3.1、规范

* 说明：专门用于浏览器端，模块的加载是异步的，模块使用时才会加载执行
* 基本语法：
    * 定义暴露模块、引入使用模块
    * ![](https://gitee.com/huqian025/my-images/raw/master/js/js%E6%A8%A1%E5%9D%97%E5%8C%96%E8%A7%84%E8%8C%83/CMD.png)

##### 3.2、实现（浏览器端）

* Sea.js（停止维护）

#### 4、ES6（掌握）

##### 4.1、规范

* 说明：依赖模块需要编译打包处理
* 语法：
    * 导出模块：export
    * 引入模块：import

##### 4.2、实现（浏览器端）

* 核心：
    * 使用Babel将ES6编译为ES5代码
    * 使用Browserify编译打包js
* 使用：
    * 安装babel-cli，babel-preset-es2015和browserify
        * npm install babel-cli browserify -g
        * npm install babel-preset-es2015 --save-dev
    * 定义 .babelrc 文件
```json
{
    "presets": ["es2015"]
}
```

* 编译
    * 使用Babel将ES6编译为ES5代码（但还是包含CommonJS语法）：babel js/src -d js/build
    * 使用Browserify编译js：browserify js/build/main.js -o js/dist/bundle.js

```javascript
// module1.js
// 常规暴露-分别暴露
export function foo () {
  console.log('foo() module1')
}
export function bar () {
  console.log('bar() module1')
}
export let arr = [1,2,3,4,5]

// module2.js
// 常规暴露-统一暴露
function fun () {
  console.log('fun() module2')
}
function fun2 () {
  console.log('fun2() module2')
}
export {fun, fun2}

// module3.js
// 默认暴露，可以暴露任意数据类型，暴露什么数据接收到的就是什么数据
export default {
  msg: '默认暴露',
  foo () {
    console.log(this.msg)
  }
}

// main.js
// 引入模块
// 常规暴露必须使用解构赋值的形式引入
import {foo, bar} from './module1'
import { fun, fun2 } from './module2'
// 默认暴露直接引入
import module3 from './module3'
// 引入第三方库类似默认暴露，以jquery为例
import $ from 'jquery'
$('body').css('background', 'green')
foo()
bar()
fun()
fun2()
module3.foo()

// html中引入
<!-- 引入通过babel和browserify打包处理后的js文件 -->
<script src="./js/dist/bundle.js"></script>
```

### 三、ES6引入export default导出的模块，不使用{}

![](https://gitee.com/huqian025/my-images/raw/master/js/js%E6%A8%A1%E5%9D%97%E5%8C%96%E8%A7%84%E8%8C%83/ES6%E4%BD%BF%E7%94%A8%E9%BB%98%E8%AE%A4%E5%AF%BC%E5%87%BA%E6%A8%A1%E5%9D%97.png)

