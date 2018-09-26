# 使用PostgreSQL

下載頁面:[http://www.enterprisedb.com/products-services-training/pgdownload](http://www.enterprisedb.com/products-services-training/pgdownload)

# 安裝

[https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/](https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/)

```
1.下載
apt-get install postgresql postgresql-contrib

2.然後設定開機自動啟動
update-rc.d postgresql enable

3.啟動postgresql
service postgresql start
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

## 1.使用pgAdmin III

如要查看TABLE

```
點選數據庫=>架構=>數據表
```

如要查看row

```
點選數據表下的table名稱，在點選最上方工具(T)下面的表格圖案
```

## 2.使用SQL shell\(psql\)

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

