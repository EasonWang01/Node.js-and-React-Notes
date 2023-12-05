# 錯誤處理

## uncaughtException 與 unhandledRejection

1. uncaughtException 發生在同步執行時未被處理的錯誤，例如 await func() 沒有用 try catch 包住。
2. unhandledRejection 發生在異步處理時的錯誤，例如 promise.then 後面沒寫 .catch

這兩者發生時都會讓程式結束執行，除非使用 PM2 或是在 docker compose 的 config 將 restart 設為 always 等。

而在程式中，可以如下來處理所有未捕獲異常狀態

```javascript
const process = require('node:process');

process.on('unhandledRejection', (reason, promise) => {
  console.log('Unhandled Rejection at:', promise, 'reason:', reason);
  // Application specific logging, throwing an error, or other logic here
});
```

> 但建議在測試環境不要使用，因為要盡可能找出所有可能發生之錯誤
