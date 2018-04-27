# Inject script 到任何網頁

Manifest.json

```json
  "content_scripts": [
    {
      "matches": ["https://*/*", "http://*/*"],
      "js": ["./src/js/inject.js"]
    }
  ]
```



