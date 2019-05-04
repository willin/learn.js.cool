# 常用正则

## 正则匹配

### 手机号码

规则：

* 11位数字 `/^\d{11}$/`
* 1开头 `/^1\d{10}$/`
* 第二位可能出现的是3456789中的一个 `[3-9]{1}`

正则表达式： `/^1[3-9]{1}\d{9}$/`

## 正则替换

### 格式化手机号

将手机号显示为 `+86.132-1234-1234` 格式。

输入： 11位手机号，如 13212341234 输出： 16位字符串

思路：

* 将手机号按照 3-4-4 进行拆分 `/\d{3}\d{4}\d{4}/`
* 正则表达式： `/^(\d{3})(\d{4})(\d{4})$/`

示例代码：

```javascript
const formatMobile = mobile => `${mobile}`.replace(/^(\d{3})(\d{4})(\d{4})$/, '+86.$1-$2-$3');

formatMobile(13212341234) // => "+86.132-1234-1234"
```
