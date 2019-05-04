---
description: MySQL不使用自增id，兼容异地分布。
---

# 随机数字id

### 创建临时id表

```sql
CREATE TABLE `t_tempid` (
  `id` int(11) unsigned NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 临时id生成脚本

```javascript
#! /usr/bin/env node

const program = require('commander');
const fs = require('fs');
const path = require('path');
const { QUERY } = require('.@lib/orm');

const TABLE = 'DATABASE.t_id';

const range = val => val.split('..').map(Number);
program
  .version('1.0.0')
  .usage('[options]')
  .option('-r, --range <a>..<b>', 'Range 1..2 means 10000-19999', range)
  .parse(process.argv);
if ((!program.range || program.range.length !== 2)) {
  program.help();
  process.exit();
}

const filename = path.join(__dirname, '../backup/', `id-${program.range.join('-')}.csv`);

const fd = fs.openSync(filename, 'w');
let block = '';
for (let i = program.range[0] * 10 ** 4; i < program.range[1] * 10 ** 4; i += 1) {
  block += `${i}\n`;
  if (i % 1000 === 0 || i === program.range[1] * 10 ** 4 - 1) {
    fs.writeSync(fd, block);
    block = '';
  }
}
fs.closeSync(fd);

(async () => {
  try {
    await QUERY(`LOAD DATA LOCAL INFILE '${filename}' INTO TABLE ${TABLE}`);
  } catch (e) {
    console.trace(e);
  }
  process.exit();
})();
```

### 获取临时id并删除

```javascript
const { isEmpty } = require('@xibang/node-common');

const { QUERY, format } = require('.@lib/orm');

const TABLE = 'DATABASE.t_id';

exports.getTempId = async () => {
  const sql = format('SELECT t1.id FROM ?? AS t1 JOIN (SELECT ROUND(RAND() * ((SELECT MAX(id) FROM ??)-(SELECT MIN(id) FROM ??))+(SELECT MIN(id) FROM ??)) AS id) AS t2 WHERE t1.id >= t2.id ORDER BY t1.id LIMIT 1', [TABLE, TABLE, TABLE, TABLE]);
  const result = await QUERY(sql);
  if (isEmpty(result)) {
    return 0;
  }
  const id = result[0].id;
  await QUERY(format('DELETE FROM ?? WHERE id = ? LIMIT 1', [TABLE, id]));
  return id;
};
```





