---
title: vue计算属性computed和侦听器watch
date: 2022-01-06 11:39:37
categories: vue
tags:
- vue
---

### 一、计算属性computed

#### 1、计算属性的setter和getter
* 每个计算属性都包含一个setter和一个getter
* 计算属性一般没有set方法，只读属性
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

