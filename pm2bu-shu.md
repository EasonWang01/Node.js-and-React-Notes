# 進程管理工具



設定環境變數\`NODE\_ENV\`

需要先新增一個\`process.json\`檔案

```
{
  "name" : "handi",
  "script" : "index.js",
  "env_production" : {
    "NODE_ENV": "production"
  }
}
```

之後執行\`pm2 start process.json --env production\`

