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

{% embed url="https://sequelize.org/docs/v6/other-topics/migrations/" %}

## DB Seeder

建立初始化資料使用，會把資料輸入 DB

在 migrations 資料夾旁邊建立 seeders 資料夾

裡面放入檔案 20240322061229-initial-tasks-campaigns.js&#x20;

```javascript
'use strict';

/** @type {import('sequelize-cli').Migration} */
module.exports = {
  async up(queryInterface, Sequelize) {
    const campaignId = 1;

    await queryInterface.bulkInsert('Campaigns', [
      {
        id: campaignId,
        name: 'Yield Pass Phase-1',
        createdAt: Sequelize.literal('NOW()'),
        updatedAt: Sequelize.literal('NOW()'),
      },
    ]);

    await queryInterface.sequelize.query(`
      INSERT INTO "Tasks" ("campaignId", "createdAt", "updatedAt", title, details) VALUES
      (1, NOW(), NOW(), 'test', 'test');
    `);

    await queryInterface.sequelize.query(`
      INSERT INTO "Rewards" ("taskId", category, points, "createdAt", "updatedAt") VALUES
      (1, 'test', 200, NOW(), NOW());
    `);
  },

  async down(queryInterface) {
    await queryInterface.bulkDelete('Rewards', null, {});
    await queryInterface.bulkDelete('Tasks', null, {});
    await queryInterface.bulkDelete('Campaigns', null, {});
  },
};
```

之後可執行全部

```javascript
npx sequelize-cli db:seed:all
```

或指定檔案

```javascript
npx sequelize db:seed --seed temp-seeder.js
```

也可以 undo

```
npx sequelize-cli db:seed:undo
```
