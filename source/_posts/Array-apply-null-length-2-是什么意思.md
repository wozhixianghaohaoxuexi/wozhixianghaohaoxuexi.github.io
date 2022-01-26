---
title: 'Array.apply(null, { length: 2 })是什么意思'
date: 2022-01-26 11:56:41
categories:
  - js
tags:
  - js
  - vue
---

看到vue文档的示例中，使用以下代码创建了2个相同的段落
>注：文档中length的值为20，这里方便演示改为了2
```js
render: function (createElement) { 
    return createElement('div', 
        Array.apply(null, { length: 2 }).map(function () { 
            return createElement('p', 'hi') 
        }) 
    ) 
}
```

#### 1. apply方法中第二个参数{length: 2}是什么意思

**对象{length: 2}是一个类数组对象，因为没有初始化下标0，1的值，所以获取0，1下标的值得到的都是undefined。**

```js
// 类数组对象可以转成真正的数组
const a = Array.prototype.slice.call({length: 2});
console.log(Array.isArray(a)) // true
```

#### 2. Array.apply(null, { length: 2 })是什么意思

**ES5开始apply函数的第二个参数除了可以是数组外，还可以是类数组对象（即包含length属性，且length属性值是个数字的对象）**

直接调用Array函数和new方式调用是等价的
```js
cosnt a = Array(2) // 等价于const a = new Array(2)
```

因此Array.apply(null, { length: 2 })就是创建一个存储两个undefined的数组
```js
Array.apply(null, { length: 2 })
// 等价于
Array.apply(null, [undefined, undefined])
// 等价于
Array(undefined, undefined)
// 等价于
new Array(undefined, undefined)
```

#### 3. Array(2)与Array.apply(null, { length: 2 })的区别

* 相同点：都是创建一个长度为2的数组
* 不同点：**Array(2)创建的数组元素没有被初始化，Array.apply(null, { length: 2 })创建的数组每个元素都被赋值为undefined。**

![创建数组](https://gitee.com/huqian025/my-images/raw/master/js/Array.apply(null,%20%7B%20length%202%20%7D)%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D/%E5%88%9B%E5%BB%BA%E6%95%B0%E7%BB%84.png)

#### 4. vue文档中为什么用Array.apply(null, { length: 2 })

**map函数不会遍历数组中没有初始化或者被delete的元素**（有相同限制还有forEach, reduce方法），根据上述“Array(2)与Array.apply(null, { length: 2 })的区别”可以得知，Array(2)创建的数组map的回调函数不会执行，Array.apply(null, { length: 2 })创建的数组map的回调函数才会执行两次

![调用map](https://gitee.com/huqian025/my-images/raw/master/js/Array.apply(null,%20%7B%20length%202%20%7D)%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D/%E8%B0%83%E7%94%A8map.png)
