# 使用PostgreSQL

## 使用PostgreSQL

下載頁面:[http://www.enterprisedb.com/products-services-training/pgdownload](http://www.enterprisedb.com/products-services-training/pgdownload)

## 目前建議用 docker 快速架設

使用 docker-compose.yml

> 一樣要建立 volumn 避免重啟後資料消失

```text
# Use postgres/example user/password credentials
version: '3.1'

services:

  db:
    image: postgres
    restart: always
    volumes:
      - "./dbdata:/var/lib/postgresql/data"
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: superuser
      POSTGRES_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

[https://hub.docker.com/\_/postgres](https://hub.docker.com/_/postgres)

## 安裝

[https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/](https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/)

```text
1.下載
apt-get install postgresql postgresql-contrib

2.然後設定開機自動啟動
sudo update-rc.d postgresql enable

3.啟動postgresql
sudo service postgresql start
```

之後輸入以下進入postgres使用者

```text
sudo -u postgres -i
```

進入psql

```text
psql
```

更改預設密碼，進入psql後輸入以下

```text
\password postgres
```

### Mac安裝

```text
brew install postgres
pg_ctl -D /usr/local/var/postgres start
createdb mydb
psql mydb // 進入DB
```

[https://medium.com/@Umesh\_Kafle/postgresql-and-postgis-installation-in-mac-os-87fa98a6814d](https://medium.com/@Umesh_Kafle/postgresql-and-postgis-installation-in-mac-os-87fa98a6814d)

### 1.使用pgAdmin

[https://www.pgadmin.org/download/](https://www.pgadmin.org/download/)

一個可視化的網頁瀏覽工具

> 如果使用本地連線時輸入localhost比較好，因為有時127.0.0.1無作用
>
> 如果出現can't connect application server，重新啟動一次即可。

**如要查看TABLE**

```text
點選數據庫=>架構=>數據表
```

![](/assets/Screen%20Shot%202018-09-26%20at%202.09.48%20PM.png)

**如要查看row**

```text
點選數據表下的table名稱，在點選最上方工具(T)下面的表格圖案
```

**執行SQL**

點選 Tools 然後按 Query Tool

![](/assets/Screen%20Shot%202018-09-26%20at%202.17.31%20PM.png)

### PgAdmin連線遠端主機的DB

1.設定pg\_hba.conf

```text
sudo vim /etc/postgresql/9.5/main/pg_hba.conf

然後加入以下

host all all 0.0.0.0/0  md5
```

> 分別為DB, User, IP 與方法

2.設定postgresql.conf

```text
listen_addresses = '*'
```

3.重啟：

```text
sudo service postgresql restart
```

4.開啟遠端主機防火牆，port：5432

然後即可使用PgAdmin遠端連線。

可參考：[https://cloud.google.com/community/tutorials/setting-up-postgres](https://cloud.google.com/community/tutorials/setting-up-postgres)

### 2.使用SQL shell \( psql \)

> 輸入psql &lt;資料庫名稱&gt;

```text
記得每個指令結尾要加 ;
使用 \q 離開。
```

```text
指令類似於 MySQL 的 SQL 指令
```

## 使用Log

> 預設只會存在psql 操作的log

log路徑：`/var/log/postgresql/postgresql-10-main.log`

1.首先要設定config: `/etc/postgresql/10/main/postgresql.conf`

2.

```text
- change the log_statement setting to 'all'.
- turned on the log_destination
- turn on the logging_collector
```

3.重啟config

```text
進入psql後輸入
SELECT pg_reload_conf();
```

參考至:

[http://www.postgresql.org/docs/8.3/static/tutorial-table.html](http://www.postgresql.org/docs/8.3/static/tutorial-table.html)

中文版:

[http://twpug.net/docs/postgresql-doc-8.0-zh\_TW/tutorial-populate.html](http://twpug.net/docs/postgresql-doc-8.0-zh_TW/tutorial-populate.html)

