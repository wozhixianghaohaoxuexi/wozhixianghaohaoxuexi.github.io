---
title: ts数据类型
date: 2022-02-24 16:10:15
categories:
  - ts
tags:
---

#### 1. number

* 所有数字都是浮点数，类型为 number，支持二进制、八进制、十进制和十六进制

```ts
// 十进制
let decimal: number = 6;
// 十六进制
let hex: number = 0xf00d;
// 二进制
let binary: number = 0b1010;
// 八进制
let octal: number = 0o744;
```

#### 2. string
```ts
let color: string = "blue";
color = 'red';
```

#### 3. boolean
```ts
let isDone: boolean = false;
```

#### 4. enum
```ts
enum Color {
    Red,
    Green,
    Blue
}
let c: Color = Color.Green;
```

#### 5. array

* 数组定义方式一：`元素类型[]`
* 数组定义方式二：`Array<元素类型>`

```ts
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

#### 6. tuple

* 元组，表示已知元素数量和类型的数组

```ts
let x: [string, number] = ["hello", 10];
```

#### 7. any

* 表示任意类型。会关闭类型检测，不建议使用

```ts
let d: any = 4;
d = 'hello';
d = true;
```

#### 8. null 和 undefined
```ts
let n: null = null;
let u: undefined = undefined;
```

#### 9. unknown

* unknown 和 any 的主要区别在于 unknown 在操作前会对类型进行检查，而 any 不会进行类型检查
* 所有类型都可以赋值给 unknown，unknown 类型只能赋值给 any 和 unknown 类型

```ts
let notSure: unknown = 4;
notSure = 'hello';
```

#### 10. void

* 表示没有任何类型。通常一个函数没有返回值时，其返回值类型为 void

```ts
let unusable: void = undefined;
```

#### 11. never

* 表示永远不存在的值的类型。never类型是那些`总是会抛出异常`或`根本就不会有返回值`的函数表达式或箭头函数表达式的返回值类型
* never 和 null 的区别在于 void 可以被赋值为 null 和undefined 类型，never 则是一个不包含值的类型。拥有 void 返回值类型的函数能正常运行，拥有 never 返回值类型的函数无法正常返回，无法终止，或会抛出异常

```ts
function error(message: string): never {
    throw new Error(message);
}
```

#### 12. symbol

* ES6 中新增数据类型，用来表示独一无二的值
* 在使用 symbol 时，必须在 tsconfig.json 的 libs 字段中 ES 版本至少为 ES2015 或 ES6

```ts
const sym1:symbol = Symbol('hello');
const sym2:symbol = Symbol('hello');
console.log(sym1 === sym2); // false
```

#### 13. bigint

* ES11 中新增数据类型，可以安全地存储和操作大整数
* 在使用 bigint 时，必须在 tsconfig.json 的 libs 字段中 ES 版本至少为 ES2020

```ts
const max1 = Number.MAX_SAFE_INTEGER;   // 2**53-1
console.log(max1 + 1 === max1 + 2); // true
const max2 = BigInt(Number.MAX_SAFE_INTEGER); 
console.log(max2 + 1n === max2 + 2n);   // false
```

#### 14. object、Object 和 {} 类型

* object 用于表示非原始类型，也就是除number、string、boolean、symbol、bigint、null 或 undefined 之外的类型
* Object 和 {} 一样，代表所有拥有 toString、hasOwnProperty 方法的类型。所有原始类型、非原始类型都可以赋给 Object（严格模式下 null 和 undefined 不可以）

```ts
// object 类型
let objectCase: object;
objectCase = 1; // error
objectCase = "a"; // error
objectCase = true; // error
objectCase = null; // error
objectCase = undefined; // error
objectCase = {}; // ok
// Object 类型
let ObjectCase: Object;
ObjectCase = 1; // ok
ObjectCase = "a"; // ok
ObjectCase = true; // ok
ObjectCase = null; // error
ObjectCase = undefined; // error
ObjectCase = {}; // ok
// {} 类型
let simpleCase: {};
simpleCase = 1; // ok
simpleCase = "a"; // ok
simpleCase = true; // ok
simpleCase = null; // error
simpleCase = undefined; // error
simpleCase = {}; // ok
```

#### 15. 字面量

* 可以使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围

```ts
let color: 'red' | 'blue' | 'black' = 'red';
let num: 1 = 1;
```

#### 15. 类型断言

* 有些情况下，变量的类型对于我们来说是很明确，但是TS编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式：

```ts
// 1. as 语法
let someValue: unknown = "this is a string";
let strLength: number = (someValue as string).length;
// 2. 尖括号语法
let someValue: unknown = "this is a string";
let strLength: number = (<string>someValue).length;
```

> 以上两种方式虽然没有任何区别，但是尖括号格式会与 react 中 JSX 产生语法冲突，因此更推荐使用 as 语法

#### 16. 非空断言

* 使用 `!` 可以用于断言操作对象是非 null 和非 undefined 类型 

```ts
let flag: null | undefined | string;
flag!.toString();   // ok
flag.toString();    // error
```

#### 17. 类型别名

* 给一个类型起一个新名字

```ts
type flag = string;
// type flag = string | number;
function fn(value: flag) {}
```

#### 18. 联合类型

* 联合类型表示取值可以为多种类型中的一种。未赋值时联合类型上只能访问两个类型共有的属性和方法

```ts
let name: string | number;
console.log(name.toString());   // 报错，name 赋值前不能使用
name = 1;
console.log(name.toFixed(2));   // 1.00
name = 'hello';
console.log(name.length);   // 5

function fn(name: string | number): void {
  name.toString();
  name.toFixed(2);  // 报错，name 上不存在 toFixed 方法
  name.length;  // 报错，name 上不存在 length 属性
}
```

#### 19. 交叉类型

* 交叉类型是将多个类型合并为一个类型。通过 & 运算符可以将现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性

```ts
type Flag1 = { x: number };
type Flag2 = Flag1 & { y: string };

let flag3: Flag2 = {
  x: 1,
  y: "hello"
};
```

> 参考资料：
> [掘金：最全的TypeScript学习指南](https://juejin.cn/post/7031787942691471396#heading-21)
> [TypeScript 官方文档](https://www.tslang.cn/docs/handbook/basic-types.html)
