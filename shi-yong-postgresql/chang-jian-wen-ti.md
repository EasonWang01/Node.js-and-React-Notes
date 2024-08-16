---
description: 常見問題
---

# 常見問題

## postgresql duplicate key violates unique constraint

當 primary key 為 auto increment 時，手動新增或移除某個 row 導致後續無法新增資料，可以使用以下 SQL 更新 primary key id 欄位

> 以下以 UserAddresses table 為例子

```
SELECT setval('"UserAddresses_id_seq"', (SELECT MAX(id) FROM "UserAddresses")+1);
```

[https://stackoverflow.com/a/64822533](https://stackoverflow.com/a/64822533)
