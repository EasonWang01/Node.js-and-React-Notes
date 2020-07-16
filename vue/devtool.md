# Devtool

## 使用

先裝好 chrome extension 後在 code裡面加上如下即可

```text
Vue.config.devtools = true;
```

## 偵錯

時常會發生有偵測到Vue  但沒顯示 devtool的 情況。

在 console 輸入以下然後重新整理即可。

```text
window.__VUE_DEVTOOLS_GLOBAL_HOOK__.Vue = app.constructor
```

