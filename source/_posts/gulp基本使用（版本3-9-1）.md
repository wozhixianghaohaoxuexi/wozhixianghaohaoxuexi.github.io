---
title: gulp基本使用（版本3.9.1）
date: 2022-01-06 11:44:35
categories: 打包工具
tags:
- glup
---

**以3.9.1版本为例，4.x版本使用参照官方文档。3.9.1不支持ES6**

### 一、入门介绍

* gulp介绍
    * gulp是与grunt功能类似的前端项目构建工具，也是基于nodejs的自动任务运行器
    * 能自动化地完成javascript/coffee/sass/less/html/img/css等文件的合并、压缩、检查、监听文件变化、浏览器自动刷新、测试等任务
    * gulp相对于grunt更高效（异步多任务），更易于使用，插件高质量

* gulp特点：任务化、基于流（I/O数据流）、异步/同步

使用：
* 安装nodejs，查看版本：node -v
* 创建一个简单应用gulp_test
![gulp_test目录结构](https://gitee.com/huqian025/my-images/raw/master/%E6%89%93%E5%8C%85%E5%B7%A5%E5%85%B7/gulp%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/gulp_test%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)

* 安装gulp：
    * 全局安装gulp：`npm install gulp -g`
    * 局部安装gulp：`npm install gulp --save-dev`
* 配置：gulpfile.js
```javascript
// 引入gulp模块
var gulp = require('gulp');
// 注册任务，注册任务后可以通过 `gulp 任务名` 执行任务
gulp.task('任务名', function () {
    // 配置任务操作
})
// 注册默认任务，可以将多个任务放在数组中，通过 `gulp` 执行多个任务
gulp.task('default', ['任务名'])
```

### 二、任务构建

#### 1、构建js任务（gulp-uglify）

* 下载插件：`npm install gulp-concat gulp-uglify gulp-rename --save-dev`
* 配置编码：
```javascript
// 引入gulp模块
var gulp = require('gulp');
// 引入gulp插件，需要先npm安装，它们都是一个方法，直接在名称后面加括号进行调用，不用通过gulp对象调用
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');
// 注册合并压缩js的任务
gulp.task('js', function () {
    return gulp.src('src/js/*.js') // 找到目标源文件，将数据读取到gulp内存中
        .pipe(concat('build.js')) // 合并文件
        .pipe(gulp.dest('dist/js/')) // 临时输出文件到本地
        .pipe(uglify()) // 压缩文件
        .pipe(rename({suffix: '.min'})) // 重命名，也可以不用对象直接写文件名
        .pipe(gulp.dest('dist/js/'))
})
```

#### 2、构建less任务（gulp-less）

* 下载插件：`npm install gulp-less --save-dev`
* 配置编码：
```javascript
var less = require('gulp-less')

// 注册转换less的任务
gulp.task('less', function () {
    return gulp.src('src/less/*.less')
        .pipe(less()) // 编译less文件为css文件
        .pipe(gulp.dest('src/css/')) // 文件名不重复可以不用重命名
})
```

#### 3、构建css任务（gulp-clean-css）

* 下载插件：`npm installl gulp-clean-css --save-dev`
* 配置编码：
```javascript
var cleanCss = require('gulp-clean-css')

// 注册合并压缩css文件
gulp.task('css', function () {
    return gulp.src('src/css/*.css')
        .pipe(concat('build.css'))
        .pipe(rename({suffix: '.min'}))
        .pipe(cleanCss({compatibility: 'ie8'})) // 兼容到ie8
        .pipe(gulp.dest('dist/css/'))
})
```

#### 4、构建压缩html任务（gulp-htmlmin）

* 下载插件：`npm install gulp-htmlmin --save-dev`
* 配置编码：
```javascript
var htmlmin = require('gulp-htmlmin')

// 注册压缩html任务
gulp.task('html', function () {
    return gulp.src('index.html')
        .pipe(htmlmin({collapseWhitespace: true})) // 清除空格
        .pipe(gulp.dest('dist/')) // 注意html中引入css和js路径变化，以前是引入src下的
})
```

#### 5、执行任务异步/同步，任务之间依赖

* 使用默认任务执行多个任务：在注册任务时，不使用return多个任务是同步执行的，使用return是异步
```javascript
// 注册默认任务
gulp.task('default', ['js', 'less', 'css']) // 直接通过 `gulp` 命名执行js、less、css三个任务
```

* 任务之间的依赖，例如：less和css异步执行，css任务要在less任务执行完成后，再执行

![任务依赖](https://gitee.com/huqian025/my-images/raw/master/%E6%89%93%E5%8C%85%E5%B7%A5%E5%85%B7/gulp%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/%E4%BB%BB%E5%8A%A1%E4%B9%8B%E9%97%B4%E4%BE%9D%E8%B5%96.png)

### 三、自动刷新

#### 1、半自动（仅改动时，自动调用相应任务gulp-livereload）

* 下载插件：`npm install gulp-livereload --save-dev`
* 配置编码：
```javascript
var livereload = require('gulp-livereload')

// 注册监视任务（半自动）
gulp.task('watch', ['default'], function () {
    // 开启监听
    livereload.listen();
    // 确认监听的目标以及绑定相应的任务
    gulp.watch('src/js/*.js', ['js']); // 监听js文件的变化，变化时启动js任务
    gulp.watch(['src/css/*.css', 'src/less/*.less'], ['css']);
})

// 在每个任务后添加.pipe(livereload())，以js任务为例
gulp.task('js', function () {
    return gulp.src('src/js/*.js') // 找到目标源文件，将数据读取到gulp内存中
        .pipe(concat('build.js')) // 合并文件
        .pipe(gulp.dest('dist/js/')) // 临时输出文件到本地
        .pipe(uglify()) // 压缩文件
        .pipe(rename({suffix: '.min'})) // 重命名，也可以不用对象直接写文件名
        .pipe(gulp.dest('dist/js/'))
        .pipe(livereload()) // 实时刷新
})
```

#### 2、全自动（自动刷新gulp-connect，自动打开链接open）

##### 2.1、页面自动刷新，不用f5
* 下载插件：`npm install gulp-connect --save-dev`
* 编码配置：
```javascript
var connect = require('gulp-connect')

// 注册监视任务（全自动），跟半自动没有任何关系
gulp.task('server', ['default'], function () {
    // 配置服务器的选项
    connect.server({
        root: 'dist/',
        livereload: true, // 实时刷新
        port: 5000
    })
    // 确认监听的目标以及绑定相应的任务
    gulp.watch('src/js/*.js', ['js']); // 监听js文件的变化，变化时启动js任务
    gulp.watch(['src/css/*.css', 'src/less/*.less'], ['css']);
})

// 在每个任务后添加.pipe(connect.reload())，以js任务为例
gulp.task('js', function () {
    return gulp.src('src/js/*.js') // 找到目标源文件，将数据读取到gulp内存中
        .pipe(concat('build.js')) // 合并文件
        .pipe(gulp.dest('dist/js/')) // 临时输出文件到本地
        .pipe(uglify()) // 压缩文件
        .pipe(rename({suffix: '.min'})) // 重命名，也可以不用对象直接写文件名
        .pipe(gulp.dest('dist/js/'))
        .pipe(connect.reload()) // 实时刷新
})
```

##### 2.2、链接自动打开，不用手动输入地址

* 下载插件：`npm install open --save-dev`
* 编码配置：
```javascript
var open = require('open')

// 注册监视任务（全自动），跟半自动没有任何关系
gulp.task('server', ['default'], function () {
    // 配置服务器的选项
    connect.server({
        root: 'dist/',
        livereload: true, // 实时刷新
        port: 5000
    })
    
    // open可以自动打开指定的链接
    open('http://localhost:5000/');
    
    // 确认监听的目标以及绑定相应的任务
    gulp.watch('src/js/*.js', ['js']); // 监听js文件的变化，变化时启动js任务
    gulp.watch(['src/css/*.css', 'src/less/*.less'], ['css']);
})
```

### 四、扩展插件

#### 1、gulp-load-plugins（打包基于gulp的所有插件，不用一个个单独引入）

* 下载插件：`npm install gulp-load-plugins --save-dev`
* 配置编码：
```javascript
var $ = require('gulp-load-plugins')();

// 任务中的每个插件通过 `$.插件名`的方式引入，以js任务为例
// 插件名为 `gulp-`后面的部分，短横线改为驼峰。如：gulp-htmlmin插件名为htmlmin，gulp-clean-css插件名为cleanCss
gulp.task('js', function () {
    return gulp.src('src/js/*.js') // 找到目标源文件，将数据读取到gulp内存中
        .pipe($.concat('build.js')) // 合并文件
        .pipe(gulp.dest('dist/js/')) // 临时输出文件到本地
        .pipe($.uglify()) // 压缩文件
        .pipe($.rename({suffix: '.min'})) // 重命名，也可以不用对象直接写文件名
        .pipe(gulp.dest('dist/js/'))
        .pipe($.connect.reload()) // 实时刷新
})
```

### 五、4.x版本使用差异（支持ES6）

* 1. 创建任务不在使用task()方法，而是直接写执行函数，并将其导出
* 2. 组合任务有两种方法：series()和parallel()，允许将多个独立的任务组合为一个更大的操作。这两个方法都可以接受任意数量的任务函数或以组合的操作，并可以互相嵌套至任意深度
    * 如果需要让任务按顺序执行，使用series()方法
    * 如果希望以最大并发来运行任务，使用parallel()方法

```javascript
const { series } = require('gulp');

// `clean` 函数并未被导出（export），因此被认为是私有任务（private task）。
// 它仍然可以被用在 `series()` 组合中。 
function clean(cb) { 
    // body omitted 
    cb(); 
} 
// `build` 函数被导出（export）了，因此它是一个公开任务（public task），并且可以被 `gulp` 命令直接调用。
// 它也仍然可以被用在 `series()` 组合中。 
function build(cb) {
    // body omitted 
    cb();
}

exports.build = build; 
exports.default = series(clean, build);
```