# 常用正则表达式整理

## 正则匹配

### 手机号码

规则：
- 11位数字 `/^\d{11}$/`
- 1开头 `/^1\d{10}$/`
- 第二位可能出现的是3456789中的一个 `[3-9]{1}`

正则表达式： `/^1[3-9]{1}\d{9}$/`


## 正则替换