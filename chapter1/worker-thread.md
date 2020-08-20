# Worker Thread

{% embed url="https://nodejs.org/api/worker\_threads.html" %}

不能同時存取主程式相同變數，所以不會有 race condition 問題，透過 `postMessage` 互相溝通。

### 範例

app.js

```javascript
const { Worker } = require('worker_threads');
const path = require('path');

const worker1 = new Worker(path.resolve('./worker.js'));

worker1.on('message', (message) => {
  console.log(`Main thread got message: ${message}`);
});
worker1.postMessage('ping');
```

worker.js

```javascript
const { parentPort } = require('worker_threads');

parentPort.on('message', (message) => {
  console.log(`worker got message: ${message}`);
  parentPort.postMessage('pong');
});
```

之後執行 `node app.js`

```text
worker got message: ping
Main thread got message: pong
```

