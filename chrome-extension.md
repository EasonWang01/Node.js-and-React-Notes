# Inject script 到任何網頁

Manifest.json

> 將要執行的程式寫在`./src/js/inject.js`

```json
  "content_scripts": [
    {
      "matches": ["https://*/*", "http://*/*"],
      "js": ["./src/js/inject.js"]
    }
  ]
```

# 取得所有頁面上的xhr事件回應

```
此與chrome extension無關
```

> [https://stackoverflow.com/a/5202999/4622645](https://stackoverflow.com/a/5202999/4622645)



