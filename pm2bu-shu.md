# 進程管理工具

#### \#PM2 教學

```
npm install pm2 -g
```

#### \#常用指令

```
pm2 start app.js //啟動
npm restart <name or id>
npm reload  <name or id>和restart類似 但可以達到0downtime
```

#### 

#### \#production

[http://pm2.keymetrics.io/docs/usage/application-declaration/](http://pm2.keymetrics.io/docs/usage/application-declaration/)

設定環境變數\`NODE\_ENV\`

輸入以下指令，會產生ecosysten.config.js檔案

```
pm2 ecosystem
```

之後執行

```
 pm2 start ecosystem.config.js --env production
```



