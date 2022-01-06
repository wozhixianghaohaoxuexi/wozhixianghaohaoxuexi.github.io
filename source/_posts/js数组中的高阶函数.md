---
title: js数组中的高阶函数
date: 2022-01-06 11:34:41
categories: js
tags:
- js
- 数组
---
#### 0、什么是高阶函数
高阶函数英文叫Higher-order function。JavaScript的函数其实都指向某个变量。既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

#### 1、filter函数的使用（过滤数组）
> MDN：filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 

filter中的回调函数有一个要求：必须返回一个boolean值
true：当返回为true时，函数内部自动将这次回调的n加入到新的数组中
false：当返回为false时，函数内部会过滤掉这次的n

* 示例：获取集合中小于100的数
```js
const nums = [10, 20, 30, 111, 222, 333]
let newNums = nums.filter(function(n){
    return n<100
})
```

* 示例：js中获取素数
```js
function get_primes(arr) {
    return arr.filter(num => {
        // 1不是素数
        if(num === 1) {
            return false;
        }
        // 从2开始，取到该数的平方根即可
        for(var i=2; i<=Math.sqrt(num); i++) {
            // 如果可以整除，证明不是素数
            if(num % i === 0) {
                return false;
            }
        }
        return true;
    });
}
```

#### 2、map函数的使用（对数组每个元素进行操作）
> MDN：map()方法创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。

* 示例：将新集合中的所有数*2
```js
let new2Nums = newNums.map(function(n){
    return n*2
})
```

#### 3、reduce函数的使用（汇总数组的内容）
> MDN：reduce()方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。

* 示例：计算集合所有数据的和
```js
// new2Nums数组的和，preValue为上一次function的返回值，0为preValue初始值
new2Nums.reduce(function(preValue, n){
    return preValue + n
}, 0)

// 简化
let total = nums.filter(function(n){
    return n<100
}).map(function(n){
    return n*2
}).reduce(function(preValue, n){
    return preValue + n
}, 0)

// 进一步简化：
let total = nums.filter(n => n<100).map(n => n*2).reduce((preValue, n) => preValue +n)
```

#### 4、sort函数的使用（数组排序）
> MDN：sort()方法用[原地算法](https://en.wikipedia.org/wiki/In-place_algorithm)对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的。[深入理解字符编码](https://juejin.cn/post/6844904159314116622)

对于两个元素x和y，如果认为x < y，则返回-1，如果认为x == y，则返回0，如果认为x > y，则返回1。**sort()方法会直接对Array进行修改，它返回的结果仍是当前Array。**

* 示例：使用sort默认排序
```js
['Google', 'Apple', 'Microsoft'].sort(); // ['Apple', 'Google', 'Microsoft'];

['Google', 'apple', 'Microsoft'].sort(); // ['Google', 'Microsoft", 'apple']

//  Array的sort()方法默认把所有元素先转换为String再排序
[10, 20, 1, 2].sort(); // [1, 10, 2, 20]
```

* 示例：将数组从小到大排序 [10, 20, 1, 2]
```js
var arr = [10, 20, 1, 2];
// 正数相当于1，负数相当于-1
arr.sort((x, y) => x-y); // [1, 2, 10, 20]
```

#### 5、every函数的使用（判断数组的所有元素是否满足测试条件）
> MDN：every()方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

**若收到一个空数组，此方法在一切情况下都会返回 true。**

* 示例：
```js
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0

console.log(arr.every(function (s) {
    return s.toLowerCase() === s;
})); // false, 因为不是每个元素都全部是小写
```

#### 6、find函数的使用（查找符合条件的第一个元素，返回值）
> MDN： find()方法返回数组中满足提供的测试函数的第一个元素的值。否则返回undefined。

* 示例：
```js
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写

console.log(arr.find(function (s) {
    return s.toUpperCase() === s;
})); // undefined, 因为没有全部是大写的元素
```

#### 7、findIndex函数的使用（查找符合条件的第一个元素，返回索引）
> MDN：findIndex()方法返回数组中满足提供的测试函数的第一个元素的索引。若没有找到对应元素则返回-1。

* 示例：
```js
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.findIndex(function (s) {
    return s.toLowerCase() === s;
})); // 1, 因为'pear'的索引是1

console.log(arr.findIndex(function (s) {
    return s.toUpperCase() === s;
})); // -1
```

**findIndex与indexOf的区别**
* findindex丢进去的是一个函数，找满足函数关系的元素。
* indexof丢进去的是要找的元素，直接找元素。

#### 8、forEach函数的使用（对数组每个元素进行操作，没有返回值）
> MDN：forEach()方法对数组的每个元素执行一次给定的函数。

forEach()和map()类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。forEach()常用于遍历数组，因此，传入的函数不需要返回值。

* 示例：
```js
var arr = ['Apple', 'pear', 'orange'];
arr.forEach(console.log); // 依次打印每个元素
```

forEach方法可以接收两个参数array.forEach(function(currentValue, index, arr), thisValue)：
1. 回调函数，必需，回调函数中有三个参数
   1. currentValue，必需，当前元素
   2. index，可选，当前元素的索引值
   3. arr，可选，当前元素所属的数组对象
2. thisValue，可选，传递给参数的值一般用“this”值，如果这个参数为空，“undefined”会传递给“this”值简单点来说，就是我们可以直接使用第二个参数来指定函数里的this的值，而不需要使用箭头函数或者在外面定义var that = this;等操作。 在加上第二个参数前，forEach函数里的this默认是指向window的，在加了第二个参数this之后则指向forEach函数所在的对象了。
![forEach方法第二个参数thisValue](https://gitee.com/huqian025/my-images/raw/master/js/js%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0/foreach%E7%AC%AC%E4%BA%8C%E4%B8%AA%E5%8F%82%E6%95%B0.png)

