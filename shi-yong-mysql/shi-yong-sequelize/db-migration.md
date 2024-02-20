# DB Migration

在開發中使用 sync 可以方便快速修改表結構，但部署環境建議使用 Migration up 與 Migration down。

## 安裝

首先安裝

```
npm install sequelize-cli
```

初始化

```
npx sequelize-cli init
```

會產生如下資料夾

<figure><img src="../../.gitbook/assets/截圖 2024-02-20 下午2.25.30.png" alt=""><figcaption></figcaption></figure>

在 config 資料夾內把  config.json 改為 config.js

```
require("dotenv").config();
const { DB_USER, DB_PASS, DB_HOST } = process.env;

module.exports = {
  uat: {
    username: DB_USER,
    password: DB_PASS,
    database: "test-db",
    host: DB_HOST,
    dialect: "mysql",
  },
  prd: {
    username: DB_USER,
    password: DB_PASS,
    database: "test-db",
    host: DB_HOST,
    dialect: "mysql",
  },
};
```

產生範例 migration script

```
sequelize-cli migration:create --migrations-path ./database/migration --name
```

之後去修改上面產生的 script 檔案

```javascript
'use strict';

/** @type {import('sequelize-cli').Migration} */
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.addColumn('Users', 'isMale', {
      type: Sequelize.BOOLEAN,
      allowNull: false,
      defaultValue: false,
    });
    await queryInterface.addColumn('Users', 'count', {
      type: Sequelize.TINYINT.UNSIGNED,
      allowNull: false,
      defaultValue: 0,
    });
  },

  async down(queryInterface) {
    await queryInterface.removeColumn('Users', 'isMale');
    await queryInterface.removeColumn('Users', 'count');
  }
};
```

執行 migration

```
NODE_ENV=uat npx sequelize-cli db:migrate --migrations-path ./database/migration
```

取消 migration

```json
NODE_ENV=uat npx sequelize-cli db:migrate:undo --migrations-path ./database/migration
```

```
> sequelize-cli 會自動在 db 新增一張 sequlizeMeta 表，用來記錄執行過的 migratiion script
```

[https://sequelize.org/docs/v6/other-topics/migrations/](https://sequelize.org/docs/v6/other-topics/migrations/)
