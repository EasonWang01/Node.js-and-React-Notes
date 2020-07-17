# Devtool

時常會發生有偵測到Vue  kc

Vue 但沒顯示 devtool的情況。







在 console 輸入以下然後重新整理即可。

```text
window.__VUE_DEVTOOLS_GLOBAL_HOOK__.Vue = app.constructor
```

