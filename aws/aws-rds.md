# AWS RDS

關聯式資料庫，可選擇 MySQL, MariaDB 等等。

![](<../.gitbook/assets/截圖 2022-08-18 上午10.42.45.png>)

等到建制程序都跑完會顯示如下：（此時可連線）

![](<../.gitbook/assets/截圖 2022-08-18 上午10.41.27.png>)

## 連線：

mysql -h \<host> -P 3306 -u admin -p

## 設置公開連線

建置好預設不能公開連線：需要開啟 security group 3306 port，使設置公開連線為是。

![](<../.gitbook/assets/截圖 2022-08-18 上午10.38.22.png>)

## 如果用 Postgres 15 版本之後要設定以下

照以下連結設置 rds.force\_ssl 1 為 0，不然會出現以下錯誤

```
error: no pg_hba.conf entry for host
```

[https://stackoverflow.com/a/77787495](https://stackoverflow.com/a/77787495)
