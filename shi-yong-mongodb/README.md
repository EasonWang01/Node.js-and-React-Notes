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

## 使用 cloud mongo

> 500GB FREE

[https://cloud.mongodb.com/](https://cloud.mongodb.com/)

