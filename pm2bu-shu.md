# 進程管理工具

http://pm2.keymetrics.io/docs/usage/cluster-mode/

#### \#PM2 教學

```
npm install pm2 -g
```

#### \#常用指令

```
pm2 start app.js //啟動

pm2 restart <name or id>

pm2 reload  <name or id>和restart類似 但可以達到0downtime

pm2 monit //啟動pm2的即時監控視窗
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



