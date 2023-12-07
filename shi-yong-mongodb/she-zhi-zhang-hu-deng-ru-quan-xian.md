---
description: 設置帳戶登入權限
---

# 設置帳戶登入權限

## 帳號密碼驗證

使用帳號密碼保護 MongoDB 服務，首先啟用 MongoDB 的認證機制。

包含以下步驟：

1. **創建管理員用戶**：在 MongoDB 的 admin 數據庫中創建一個具有管理員權限的用戶。
2. **配置 MongoDB 以使用認證**：修改 MongoDB 的配置文件以啟用認證。
3. **重新啟動 MongoDB 服務**：使用更新過的配置重啟 MongoDB 服務。

#### 步驟 1：創建管理員用戶

首先，啟動 MongoDB 服務而不帶認證，然後連接到 MongoDB shell 並創建一個管理員用戶：

```sh
mongo
```

在 MongoDB shell 中運行：

```javascript
use admin
db.createUser({
  user: "admin",
  pwd: "adminPassword",
  roles: [{ role: "root", db: "admin" }]
})
```

這會在 admin 數據庫中創建一個名為 "admin" 的用戶，密碼為 "adminPassword"，並賦予管理所有數據庫的權限。

#### 步驟 2：配置 MongoDB 以使用認證

在 MongoDB 配置文件中（通常是 `mongod.conf`），啟用認證。配置文件的位置取決於安裝方式和操作系統。一般情況下，可以在 `/etc/mongod.conf` 或 `/usr/local/etc/mongod.conf` 中找到。

在配置文件中添加以下：

```yaml
security:
  authorization: "enabled"
```

#### 步驟 3：重新啟動 MongoDB 服務

保存配置文件後，重新啟動 MongoDB 服務以應用這些更改。使用以下命令：

```sh
sudo mongod --config /path/to/mongod.conf --dbpath /usr/local/var/mongodb
```

> 執行完上面命令後，通常不會看到任何 console 為正常，記得將 `/path/to/mongod.conf` 替換為配置文件的實際路徑。

現在，每次連接到 MongoDB 時，都需要提供用戶名和密碼。例如，使用 MongoDB shell 連接時，可以使用以下命令：

```sh
mongo -u admin -p adminPassword --authenticationDatabase admin
```

之後程式的連線 uri 為如下

```
mongodb://admin:adminPassword@localhost:27017/<db name>?authSource=admin
```
