# 数组

### 生成一个 m 到 n 的数组

用 ES6 的 Fill 特性可以避免 `new Array(N)` 无法被 Map 的问题。

```js
const list = (m, n) =>
  new Array(n - m + 1).fill(0).map((_, i) => m + i);
```
