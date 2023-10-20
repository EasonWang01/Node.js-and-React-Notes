# 使用MongoDB

## docker 快速設環境

docker-compose.yml

```yaml
# Use root/example as user/password credentials
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    volumes:
      - "./datadir:/data/db"
    ports:
      - 27017:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: example
```

> mongo-express 為網頁視覺 DB 資料介面，但其預設沒提供帳號登入的頁面

## 初始化資料庫

#### 方法 1

mongo 的 docker image 預設執行所有在 docker-entrypoint-initdb.d 內的腳本

```javascript
Any files with a .sh extension will be executed as shell scripts.
Any files with a .js extension will be executed as JavaScript files using the MongoDB shell.
Any files with a .json extension will be imported into the database using mongoimport.
```

所以可以把腳本放在 ./mongodb 資料夾內，之後 image 啟動後都會被執行。

```javascript
dockercompose
services:
  # mongo:
  #   image: mongo:4.4
  #   ports:
  #     - "27019:27017"
  #   environment:
  #     - MONGO_INITDB_DATABASE=eyedeal
  #     - MONGO_INITDB_ROOT_USERNAME=root
  #     - MONGO_INITDB_ROOT_PASSWORD=password
  #   volumes:
  #     - ./mongodb:/docker-entrypoint-initdb.d
  #     - ./mongodb/data:/data/db
```

#### 方法 2

test-init.js

```javascript
db = db.getSiblingDB('testDB');
db.createCollection('test1');

db.shops.insertMany([
  {
    id: '1',
    name: 'test1',
  },
  {
    id: '2',
    name: 'test2',
  },
]);
```

然後執行：

```
mongo < test-init.js
```

## 使用 cloud mongo

> 500GB FREE

{% embed url="https://cloud.mongodb.com/" %}

教學：[https://docs.mongodb.com/drivers/node/quick-start](https://docs.mongodb.com/drivers/node/quick-start)
