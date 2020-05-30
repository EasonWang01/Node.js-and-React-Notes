# 小技巧

## 快速 copy store 內的整個 object 到 clipboard

在可以存取到 store 的地方加上這段

```javascript
  Object.defineProperty(window, 'reduxStore', {
    get() {
      return store.getState();
    },
  });
```

然後到 browser console 輸入 `copy(reduxStore)`

