# 数组

<!-- toc -->

## Map 介绍

### 语法

```js
array.map(callback[, thisArg])
```

### 参数

#### callback

原数组中的元素经过该方法后返回一个新的元素。

> currentValue

callback 的第一个参数，数组中当前被传递的元素。

> index

callback 的第二个参数，数组中当前被传递的元素的索引。

> array

callback 的第三个参数，调用 map 方法的数组。

#### thisArg

执行 callback 函数时 this 指向的对象。

### 返回值

由回调函数的返回值组成的新数组。

### 例题

<https://www.codewars.com/kata/double-char>

Given a string, you have to return a string in which each character (case-sensitive) is repeated once.

```js
doubleChar("String") ==> "SSttrriinngg"

doubleChar("Hello World") ==> "HHeelllloo  WWoorrlldd"

doubleChar("1234!_ ") ==> "11223344!!__  "
```

Good Luck!

答案：

```js
const doubleChar = str => str.split('').map(i => i.repeat(2)).join('');
```


## Reduce 介绍

### 语法

```js
arr.reduce(callback,[initialValue]);
```

### 参数

#### callback

执行数组中每个值的函数，包含四个参数：

> accumulator

上一次调用回调返回的值，或者是提供的初始值（initialValue）

> currentValue

数组中正在处理的元素

> currentIndex

数据中正在处理的元素索引，如果没有提供initialValues，默认从0开始

> array

调用 reduce 的数组

#### initialValue

作为第一次调用 callback 的第一个参数。

### 返回值

函数累计处理的结果。

### 例题

<https://www.codewars.com/kata/beginner-reduce-but-grow>

Given and array of integers (x), return the result of multiplying the values together in order. Example:

```
[1, 2, 3] --> 6
```

For the beginner, try to use the reduce method - it comes in very handy quite a lot so is a good one to know.

Array will not be empty.

答案：

```js
const grow = x => x.reduce((r, i) => r * i, 1);
```


## 遍历用Map还是For

同是遍历，但实际有很大不同。

### 对比

#### map

改变自身。

```js
[1,2,3,4,5].map(x => x+1)
// [ 2, 3, 4, 5, 6 ]
```

#### for

只是循环。

### Benchmark测试

benchmark脚本：

```js
suite('iterator', function () {
  bench('for', function () {
    const a = [1, 2, 3, 4, 5];
    for (let i = 0; i < a.length; i++) {
      // nothing
    }
  });
  bench('foreach', function () {
    const a = [1, 2, 3, 4, 5];
    a.forEach(function (d) {
      // nothing
    });
  });
  bench('for of', function () {
    const a = [1, 2, 3, 4, 5];
    for (let i of a) {
      // nothing
    }
  });
  bench('map', function () {
    const a = [1, 2, 3, 4, 5];
    a.map(x => x);
  });
});
```

测试结果：

```bash
                      iterator
      50,038,931 op/s » for
       8,980,276 op/s » foreach
       8,990,758 op/s » for of
       1,713,807 op/s » map


  Suites:  1
  Benches: 4
  Elapsed: 5,710.33 ms
```

### 结论

单凭循环 `for` 最可靠。

`foreach` 和 `for ... of` 差不多。

`map` 性能最低。

## 生成一个 m 到 n 的数组

用 ES6 的 Fill 特性可以避免 `new Array(N)` 无法被 Map 的问题。

```js
const list = (m, n) =>
  new Array(n - m + 1).fill(0).map((_, i) => m + i);
```
