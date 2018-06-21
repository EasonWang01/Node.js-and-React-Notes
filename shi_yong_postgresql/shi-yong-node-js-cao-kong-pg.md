## 安裝

可使用以下模組，與資料庫連線。

```
npm install pg --save
```

[https://github.com/brianc/node-postgres](https://github.com/brianc/node-postgres)

## 執行指令前輸入相關連線設定

```
$ PGUSER=dbuser \
  PGHOST=database.server.com \
  PGPASSWORD=secretpassword \
  PGDATABASE=mydb \
  PGPORT=3211 \
  node script.js
```

## Query

```js
const { Client } = require('pg')
const client = new Client()

client.connect()

client.query('select * from company;', (err, res) => {
  console.log(err ? err.stack : res.rows)
  client.end()
})
```



