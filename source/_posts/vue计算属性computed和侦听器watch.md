---
title: vue计算属性computed和侦听器watch
date: 2022-01-06 11:39:37
categories: vue
tags:
- vue
---

### 一、计算属性 computed

#### 1、计算属性的 setter 和 getter

* 每个计算属性都包含一个 setter 和一个 getter
* 计算属性一般没有 set 方法，只读属性
* 计算属性在使用时作为一个属性，不用加()
* 注意：如果计算属性使用了箭头函数，则 this 不会指向这个组件的实例，不过仍然可以将其实例作为函数的第一个参数来访问

```javascript
var vm = new Vue({
    data: { a: 1 },
    computed: {
        // 仅读取
        aDouble: function () { 
            return this.a * 2 
        }, 
        // 读取和设置 
        aPlus: { 
            get: function () {
                return this.a + 1
            }, 
            set: function (v) { 
                this.a = v - 1 
            }
        } 
    }
})
vm.aPlus // => 2
vm.aPlus = 3 
vm.a // => 2 
vm.aDouble // => 4
```

#### 2、计算属性的缓存

* 计算属性的结果会被缓存，除非依赖的响应式 property 变化才会重新计算
* 注意：如果某个依赖 (比如非响应式 property) 在该实例范畴之外，则计算属性是不会被更新的

### 二、侦听器 watch

* watch 为一个对象，键是需要观察的表达式，值为对应的回调函数。值也可以是方法名，或者包含选项的对象
* 注意：不能使用箭头函数来定义 watcher 函数，this 指向会有问题

```js
var vm = new Vue({
    data: { 
        a: 1,
        b: 2,
        c: {
            d: 3
        }
    },
    watch: {
        // 值为一个函数
        a: function(val, oldVal) {
            console.log(`newVal: ${val}, oldVal: ${oldVal}`)
        },
        // 值可以为一个方法名
        b: 'someMethod',
        // 值为一个对象，可以配置 deep 和 immediate 属性值
        c: {
            handler: function(val, oldVal) {
                console.log(`newVal: ${val}, oldVal: ${oldVal}`)
            },
            // 深度监听，回调会在被监听对象任何属性改变时调用，不论嵌套多深
            deep: true,
            // 立即监听，回调会在监听开始时就调用一次。否则回调只在数据变化时调用
            immediate: true
        },
        // 用于监听 c.d 的值变化
        'c.d': function(val, oldVal) {
            console.log(`newVal: ${val}, oldVal: ${oldVal}`)
        }
    }
})
```

> 参考文档：
> [vue2 官方 API 选项 computed 和 watch](https://cn.vuejs.org/v2/api/#watch)
