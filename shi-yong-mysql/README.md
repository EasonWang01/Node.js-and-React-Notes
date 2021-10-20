# 使用MySQL

### Docker 執行 MySQL

stack.yml

```yaml
# Use root/example as user/password credentials
version: '3.1'

services:

  db:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    ports:
      - 3306:3306
    volumes: 
      - "./dbdata:/var/lib/mysql"  
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

之後輸入 `docker-compose -f stack.yml up`

然後可進入 localhost:8080 查看 adminer 視覺化 GUI 操作資料庫

> 帳號為 root，密碼為example

### 讀入範例資料

先在剛才外面 git clone [https://github.com/datacharmer/test\_db](https://github.com/datacharmer/test\_db)

> 記得看 yaml 的 volumn 位置

然後進到 docker 執行 sql import

```
docker exec -it <image id> sh
cd test_db
mysql -u root -pexample < employees.sql
```

之後回到 adminer 網頁，點選重新載入。即可看到多了 employees 資料庫

![](<../.gitbook/assets/螢幕快照 2020-07-24 下午4.18.53.png>)

## 使用遠端 MYSQL 免費服務

臨時免費信箱:[http://www.yopmail.com/zh/](http://www.yopmail.com/zh/)

測試用免費mysql:[https://www.db4free.net/signup.php](https://www.db4free.net/signup.php)\
(預設一個資料庫，不可再增加或修改)

## 於Linux安裝的MySQL

一開始預設只有local可以存取到本地資料夾

停止

```
/etc/init.d/apache2 start
```

開啟

```
sudo /etc/init.d/apache2 start
```

> phpmyadmin中的使用者即為mysql的使用者所以更改後兩者都會變，安裝phpmyadmin套件可參考本書GCE章節。

如在terminal輸入mysql後告知沒有權限，可輸入以下

```
mysql -u root -p
```

## Docker mysql

### 安裝

```
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
```

### 進入docker

```
docker exec -it <docker id> bash
```

### 進入 mysql cli

```
mysql -u root -p
之後輸入密碼 my-secret-pw
```

### 從外面連入 docker 進入 mysql shell

```
docker exec some-mysql sh -c 'mysql -u root -p"my-secret-pw"'
```

> 不過這時會收到警告 mysql: \[Warning] Using a password on the command line interface can be insecure.

所以可以到 \`**/etc/mysql/my.cnf**\` 設置使用者

### 使用 docker-compose 搭配 GUI(adminer)

```
version: '3.1'

services:

  db:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    volumes: ["/Users/yicheng/server/database:/db"]
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

> 帳號為 root 密碼為 example

### 執行 SQL 檔案

```
mysql -uroot -pexample < ./user.sql
```

> \-u -p 後面接的是帳號和密碼
