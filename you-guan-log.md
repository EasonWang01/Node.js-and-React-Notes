# 產生Log後寫入到檔案

這邊使用log模組

https://github.com/tj/log.js

```
yarn add log
```

```js
const fs = require('fs');
const Log = require('log');
const dir = 'logs';
if (!fs.existsSync(dir)) {
  fs.mkdirSync(dir);
}
const log = new Log('info', fs.createWriteStream(`${dir}/info_${Date.now()}.log`));
```

> 檔案位置在log資料夾下，並寫log檔案後面加上timestamp，預防重新執行程式後相同檔案名稱造成log被覆蓋。

寫入

```js
  log.info({
    message: '...',
    user: '...'
  });
  log.error({
    message: '...',
    user: '...'
  });
```

讀取

```js
var stream = fs.createReadStream(`${dir}/info_1529635751525.log`)
logReader = new Log('debug', stream);
logReader.on('line', function (line) {
  console.log(line);
});
```





