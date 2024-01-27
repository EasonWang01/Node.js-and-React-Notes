# 使用 Sequelize

## 使用 Sync 同步 table

因為有時會出現更換 server 環境後 table 不存在情況，Sequelize 會自動 sync 寫好的 model schema 到 DB table。但要記得在初始化 server 前，引入 model，順序不同可能會產生創建

table 錯誤。

> sync({force: true}) 會把表裡面的資料都刪除整個 table truncate 後重新創建 table。另外如果還沒有 table 的情況使用 sync({force: true})  可能無法正確創建，在沒有表的時候建議使用  sync({force: false}) ，如果是修改 schema 可以用 sync({alter: true})
>
> * 記得正式環境不要用 `{force: true}` &#x20;

[https://sequelize.org/docs/v7/models/model-synchronization/](https://sequelize.org/docs/v7/models/model-synchronization/)

## 程式範例：

1.創建 /database/connect.js

```javascript
const { Sequelize } = require('sequelize');
const dotenv = require("dotenv"); // Import dotenv
dotenv.config(); // Load environment variables from .env file
// Initialize Sequelize with your MySQL connection details
const { DB_USER, DB_PASS, DB_HOST } = process.env;
const sequelize = new Sequelize('db_name', DB_USER, DB_PASS, {
  host: DB_HOST,
  dialect: 'mysql',
  logging: false, 
});

module.exports = sequelize;
```

2.創建 /database/models/user.js

```javascript
const { DataTypes } = require('sequelize');
const sequelize = require('../connect');

const User = sequelize.define('User', {
  id: {
    type: DataTypes.UUID,
    defaultValue: DataTypes.UUIDV4,
    primaryKey: true,
    allowNull: false,
  },
  createdAt: {
    type: DataTypes.DATE,
    allowNull: false,
    defaultValue: sequelize.literal('CURRENT_TIMESTAMP'), // Set the default value to the current timestamp
  },
});

module.exports = User;
```

```javascript
const express = require("express");
const app = express();
const sequelize = require("./database/connect.js");
// 在這裡記得引入 schema，他才會知道到要創 table
require("./database/models/user");
require("./database/models/userReward");

async function initDatabase() {
  try {
    await sequelize.authenticate();
    await sequelize.sync({ force: false }); // {alter: true}
    console.log("Database synced")
    // Additional model-related initialization can go here
  } catch (err) {
    console.error('Failed to initialize database:', err);
    throw err;
  }
}

const initRoute = async () => {
  const userRoute = require("./router/user");
  // Router
  app.use(`/user`, userRoute);
};

const server = app.listen(port, function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log("Listening on port " + port);
  }
});


async function main() {
  try {
    await initDatabase();
    await initRoute()
  } catch (err) {
    console.error('Application initialization failed:', err);
  }
}

main();

module.exports = {
  server,
};
```

## 創建資料

```javascript
    await User.findOrCreate({
      where: { txId },
      defaults: {
        name: "..."
      },
    });
```
