# 使用Node.js操控pg



### 安裝

可使用以下模組，與資料庫連線。

```
npm install pg --save
```

[https://github.com/brianc/node-postgres](https://github.com/brianc/node-postgres)

[https://node-postgres.com/features/queries](https://node-postgres.com/features/queries)

> 可選擇使用client或使用Pool
>
> [https://node-postgres.com/features/pooling](https://node-postgres.com/features/pooling)

### 這邊預設 Pool connection 數量預設只有 10

建議修改增大，不然一次發出許多 query 會被中斷，並且 server 無法與 DB 連線，並且沒有顯示錯誤訊息。

[https://node-postgres.com/api/pool](https://node-postgres.com/api/pool)

### 執行指令前輸入相關連線設定

```
$ PGUSER=dbuser \
  PGHOST=database.server.com \
  PGPASSWORD=secretpassword \
  PGDATABASE=mydb \
  PGPORT=3211 \
  node script.js
```

或是寫在程式

```javascript
const { Client } = require('pg')
const client = new Client(
  {
    user: 'yicheng',
    host: 'localhost',
    database: 'mysb',
    password: '123456',
    port: 5432,
  }
)
```

### Query

```javascript
const { Client } = require('pg')
const client = new Client()

client.connect()

client.query('select * from company;', (err, res) => {
  console.log(err ? err.stack : res.rows)
  client.end()
})
```

### Insert

```javascript
const { Client } = require('pg')
const client = new Client()

client.connect()
var data = ["000", "bazi", 2, 1, Date.now()];
var queryletter =`INSERT INTO bet_user(ADDRESS, CATEGORY, ODDS, AMOUNT, TIMESTAMP) VALUES ($1, $2, $3, $4, $5)`;

client.query(queryletter, data, (err, res) => {
  console.log(err ? err.stack : res.rows)
  client.end()
})
```

> 如果是直接寫，記得要是value單引號，不然會出現沒有該column name的錯誤
>
> ```
>   const insertString = `INSERT INTO users (account, password, username) VALUES('${req.body.account}','${req.body.password}','${req.body.account}');`
> ```

## 使用Pool

```javascript
async function query(exec_query, data, callback) {
  const _client = await client.connect();
  if (typeof data === "function") {
    callback = data;
    data = "";
  }
  await client.query(exec_query, data, (err, res) => {
    if (err) return err;
    _client.release();
    callback(res);
  });
}
```

> 記得要release() 不然程式會當掉
>
> [https://node-postgres.com/features/pooling](https://node-postgres.com/features/pooling)

現在可以直接

```javascript
const pool = new Pool()
pool.query
```

## 存入 timestamp

```
let values = [(Date.now() + 1000 * 60 * 60 * 8) / 1000.0]

然後 query 使用

to_timestamp($1)
```

## 使用 Sequelize

```javascript
import { Sequelize, Model, DataTypes } from 'sequelize'
const sequelize = new Sequelize('postgres://superuser:example@localhost:5432/hotels') // Example for postgres

class Dog extends Model {}

Dog.init({
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  age: {
    type: DataTypes.INTEGER,
    allowNull: false
  }
}, {
  sequelize,
  modelName: 'dogs',
  timestamps: false
})

const name = 'Roger'
const age = 8
const result = await Dog.create({ name, age })
console.log(result)

Dog.findOne({
  where: {name: "Roger"}, // where 條件
  attribute: []  //指定回傳欄位
}).then(function(object) {
  console.log(object)
  if(object) {
    console.log("exist")
  } else {
    console.log("Not exist")
  }
})
```

## 如果出現 error: syntax error at or near 或是 column \_ not exist

&#x20;注意 table 名稱不要取到 SQL 保留字，例如 user 改為 users, order 要改為 orders
