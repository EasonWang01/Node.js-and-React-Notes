# JavaScript 迴圈與異步處理

## forEach

> 嚴格來說不算是回圈，因為無法使用 continue or break
>
> forEach 中 continue 對應為 return
>
> break 對應為 throw Error(...)

&#x20;foreach 內有 await，每個 foreach 不互相等待，但作用域內上一行有 await 會等待才執行下一行，等於多個 item parallel 執行。

例如：

```javascript
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function processItems(items) {
  items.forEach((item, idx) => {
      await sleep(1000);  // Simulate some delay
      console.log('Processed item:', idx);
  });
}

// Example usage
const items = [10, 20, 30, 40, 50];
processItems(items);
```

## for of

每個 iteration 對象互相等待 for of 有五個元素，會等到上一個元素的所有 await and promise 都執行完才繼續下一輪。
