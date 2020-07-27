# SQL 語法

## 引入範例資料

這邊我們使用範例資料來進行操作：

[https://github.com/datacharmer/test\_db](https://github.com/datacharmer/test_db)

## INNER JOIN

將兩個表合併，並且row只回傳符合on 的資料列

```text
select * 
from titles 
inner join employees 
on titles.emp_no=employees.emp_no
where titles.emp_no < 10100
```

> where 記得要放在最後面，然後使用 where 要指定哪個表

## LEFT OUTER JOIN

將兩個表合併，回傳左邊表所有資料（沒有的欄位資料顯示為 NULL），但右邊只回傳符合 on 的資料列

\*\*\*\*

## **RIGHT OUTER JOIN**

將兩個表合併，回傳右邊表所有資料（沒有的欄位資料顯示為 NULL），但左邊只回傳符合 on 的資料列

## Full **OUTER JOIN**

**回傳所有列資料，沒有欄位的資料顯示為 NULL**



