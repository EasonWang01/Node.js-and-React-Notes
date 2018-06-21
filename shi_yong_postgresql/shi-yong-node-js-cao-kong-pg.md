## 安裝

可使用以下模組，與資料庫連線。

```
npm install pg --save
```

[https://github.com/brianc/node-postgres](https://github.com/brianc/node-postgres)

https://node-postgres.com/features/queries

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

## Insert

```js
const { Client } = require('pg')
const client = new Client()

client.connect()
var data = ["000", "bazi", 2, 1, Date.now()];
var queryletter =`INSERT INTO bet_user(ADDRESS, CATEGORY, ODDS, AMOUNT, TIMESTAMP) VALUES ($1, $2, $3, $4, $5)`;

client.query(queryletter,data, (err, res) => {
  console.log(err ? err.stack : res.rows)
  client.end()
})
```



