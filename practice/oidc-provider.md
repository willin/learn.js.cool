---
description: 实现OIDC服务器端
---

# OIDC Provider

Github项目：[https://github.com/panva/node-oidc-provider](https://github.com/panva/node-oidc-provider)

### 生成Keys

生成脚本如下：

```javascript
const fs = require('fs');
const path = require('path');
const { createKeyStore } = require('oidc-provider');

const keystore = createKeyStore();

Promise.all([
  keystore.generate('RSA', 2048),
  keystore.generate('EC', 'P-256'),
  keystore.generate('EC', 'P-384'),
  keystore.generate('EC', 'P-521'),
]).then(() => {
  fs.writeFileSync(path.resolve('src/keystore.json'), JSON.stringify(keystore.toJSON(true), null, 2));
});
```

基本配置使用可以参考示例：[https://github.com/panva/node-oidc-provider-example/tree/master/01-oidc-configured](https://github.com/panva/node-oidc-provider-example/tree/master/01-oidc-configured)

### Adapter

{% embed url="https://github.com/panva/node-oidc-provider/tree/master/example/adapters" %}

目前有三种：mongodb、redis、redis\_rejson，其他需要自己实现。

直接下载复制到源码目录即可。

### 用户管理及Views

{% embed url="https://github.com/panva/node-oidc-provider-example/blob/master/03-oidc-views-accounts/account.js" %}

参考示例。

