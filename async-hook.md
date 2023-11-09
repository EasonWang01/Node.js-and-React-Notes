---
description: Node.js async hook
---

# Async Hook

Node.js 的`AsyncLocalStorage`類，是在`async_hooks`模組中提供的，它用於創建非同步狀態的命名空間（儲存），使得資料可以跨非同步呼叫安全地提交。以下是一些主要用途`AsyncLocalStorage`：

1. **請求層級的狀態管理**：在處理HTTP請求時，您可以用它來儲存與目前請求的相關訊息，例如使用者身份驗證、語言偏好、相關日誌等。
2. **日誌追蹤**：追蹤日誌時，透過`AsyncLocalStorage`可以為日誌添加上下文訊息，例如請求ID，這樣可以輕鬆追蹤單一請求的日誌輸出。
3. **追蹤事務**：在資料庫事務處理中，可以用於追蹤和管理請求生命週期內的資料庫連線或事務狀態。
4. **使用者會話管理**：管理使用者的會話訊息，例如在每次請求中持有使用者的認證訊息，而不是到處傳遞使用者物件。
5. **效能監控**：監控和中斷請求處理的效能，儲存開始處理請求的時間，並在請求結束時間比較時間。
6. **錯誤處理**：在非同步呼叫鏈的各個階段提交錯誤或狀態訊息，以便於統一的錯誤處理和回應。
7. **注入**：在應用程式的不同部分注入依賴，並透過每個函數傳遞以顯著方式消耗。
8. **微服務呼叫鏈追蹤**：在微服務架構中追蹤服務呼叫鏈，用於調試和監控服務間互動。

使用`AsyncLocalStorage`的一個關鍵好處是它不侵入性地提供了一種方式來調用上下文。它依賴 Node.js 的簡單異步鉤子（async hooks）來追踪非同步資源的建立和解決，從而允許儲存在`AsyncLocalStorage`實例中的資料在非同步操作間互動。

```javascript
const { AsyncLocalStorage } = require('async_hooks');
const http = require('http');

const asyncLocalStorage = new AsyncLocalStorage();

http.createServer((req, res) => {
  // 為每個請求創建特別的 id
  const context = { requestId: Date.now() + Math.random() };

  // 使用 AsyncLocalStorage.run 給異步操作創建上下文
  asyncLocalStorage.run(context, () => {
    // mock async action
    process.nextTick(() => {
      // 在此仍可以訪問上下文
      console.log('In process.nextTick:', asyncLocalStorage.getStore());
    });

    // 使用 setTimeout 模擬延遲
    setTimeout(() => {
      // 訪問上下文
      console.log('In setTimeout:', asyncLocalStorage.getStore());
      res.end(`Request handled with id: ${context.requestId}`);
    }, 100);
  });
}).listen(3000, () => {
  console.log('running');
});
```
