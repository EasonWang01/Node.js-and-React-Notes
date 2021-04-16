# SQL 基礎

[http://www.postgresqltutorial.com/](http://www.postgresqltutorial.com/)

## 不選空值 NOT NULL AND EMPTY

```sql
SELECT device_token FROM users WHERE device_token IS NOT NULL AND device_token != '';
```

## 選不重複

```sql
SELECT DISTINCT ON device_token FROM users;
```

## Data type

[https://www.w3schools.com/sql/sql\_datatypes.asp](https://www.w3schools.com/sql/sql_datatypes.asp)

## 設定column預設值

```text
ALTER TABLE <Table名稱> ALTER COLUMN <column名稱> SET DEFAULT <預設值>;
```

或是建表時設定

{% embed url="https://www.postgresql.org/docs/9.3/static/ddl-default.html" %}

## 使用變數

可以使用 with as，後面一定要接著 select 語句

{% embed url="https://stackoverflow.com/a/18010878/4622645" %}

## 計算經緯度的距離

[https://www.postgresql.org/docs/current/earthdistance.html](https://www.postgresql.org/docs/current/earthdistance.html)

完整範例：

[https://stackoverflow.com/questions/67118067/calculating-distance-then-finding-people-within-specific-range-of-given-latitude/67122368\#67122368](https://stackoverflow.com/questions/67118067/calculating-distance-then-finding-people-within-specific-range-of-given-latitude/67122368#67122368)

