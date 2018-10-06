# 使用PostgreSQL

下載頁面:[http://www.enterprisedb.com/products-services-training/pgdownload](http://www.enterprisedb.com/products-services-training/pgdownload)

# 安裝

[https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/](https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/)

```
1.下載
apt-get install postgresql postgresql-contrib

2.然後設定開機自動啟動
sudo update-rc.d postgresql enable

3.啟動postgresql
sudo service postgresql start
```

之後輸入以下進入postgres使用者

```
sudo -u postgres -i
```

進入psql

```
psql
```

更改預設密碼，進入psql後輸入以下

```
\password postgres
```

## Mac安裝

```
brew install postgres
pg_ctl -D /usr/local/var/postgres start
createdb mydb
psql mydb // 進入DB
```

[https://medium.com/@Umesh\_Kafle/postgresql-and-postgis-installation-in-mac-os-87fa98a6814d](https://medium.com/@Umesh_Kafle/postgresql-and-postgis-installation-in-mac-os-87fa98a6814d)

## 1.使用pgAdmin

[https://www.pgadmin.org/download/](https://www.pgadmin.org/download/)

一個可視化的網頁瀏覽工具

> 如果使用本地連線時輸入localhost比較好，因為有時127.0.0.1無作用
>
> 如果出現can't connect application server，重新啟動一次即可。

#### 如要查看TABLE

```
點選數據庫=>架構=>數據表
```

![](/assets/Screen Shot 2018-09-26 at 2.09.48 PM.png)

#### 如要查看row

```
點選數據表下的table名稱，在點選最上方工具(T)下面的表格圖案
```

#### 執行SQL

點選 Tools 然後按 Query Tool

![](/assets/Screen Shot 2018-09-26 at 2.17.31 PM.png)

## 2.使用SQL shell\(psql\)

> 輸入psql &lt;資料庫名稱&gt;

```
記得每個指令結尾要加 ;
使用 \q 離開。
```

```
指令如同Mysql的SQL指令
```

---

# 

參考至:

[http://www.postgresql.org/docs/8.3/static/tutorial-table.html](http://www.postgresql.org/docs/8.3/static/tutorial-table.html)

中文版:

[http://twpug.net/docs/postgresql-doc-8.0-zh\_TW/tutorial-populate.html](http://twpug.net/docs/postgresql-doc-8.0-zh_TW/tutorial-populate.html)

