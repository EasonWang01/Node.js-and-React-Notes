因為一般function沒有內建promise所以無法用Async，需要如下使用

```js
function timeout(ms) {
    return new Promise(resolve => setTimeout(() => {
    resolve();
    console.log(111)
  }, ms));
}
async function sleep() {
    await timeout(3000);
    console.log(123)
}
```



