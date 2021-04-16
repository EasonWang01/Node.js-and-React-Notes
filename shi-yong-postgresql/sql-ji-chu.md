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

## 地理位置距離計算

{% embed url="https://www.postgresql.org/docs/current/earthdistance.html" %}

> 也可參考 postGIS [https://postgis.net/](https://postgis.net/)

```sql
// 先在 SQL 輸入以下，安裝插件
create extension cube;
create extension earthdistance;

// 搜尋範例
select (point(-0.1277,51.5073) <@> point(-74.006,40.7144)) as distance;
```

[https://stackoverflow.com/a/25140461](https://stackoverflow.com/a/25140461)

![&#x8A18;&#x5F97;&#x4F7F;&#x7528; point &#x985E;&#x578B;](../.gitbook/assets/jie-tu-20210415-xia-wu-3.25.59.png)

搜尋兩人距離範例：

```sql
select (
  point(
    (select latlng from user_account where first_name = 'Manel')
  ) 
<@> 
  point(
    (select latlng from user_account where first_name = 'Ben')
  )
) as distance
```

## 計算與每個人的經緯度的距離

完整範例：

[https://stackoverflow.com/questions/67118067/calculating-distance-then-finding-people-within-specific-range-of-given-latitude/67122368\#67122368](https://stackoverflow.com/questions/67118067/calculating-distance-then-finding-people-within-specific-range-of-given-latitude/67122368#67122368)

