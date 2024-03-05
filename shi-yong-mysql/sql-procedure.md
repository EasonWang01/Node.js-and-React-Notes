# SQL Procedure

把許多的 SQL Statements 組合起來，進行一些複雜操作

> 也可以單獨對使用者加上此 sql procedure  執行權限（所以達到讓只有 read 權限的 user，具有特定 update 條件的權限）
>
> ```
> GRANT EXECUTE ON PROCEDURE database_name.procedure_name TO 'username'@'host';
> ```

## 語句

```sql
Create Procedure [Procedure Name] ([Parameter 1], [Parameter 2], [Parameter 3] )
Begin
  SQL Queries..
End

CALL [Procedure Name] ([Parameters]..)
```

## 範例：

> 下面將在範圍區間隨機產生資料

```sql
USE `db-name`;
SET FOREIGN_KEY_CHECKS=0;

DELIMITER $$

DROP PROCEDURE IF EXISTS generateMockData$$

CREATE PROCEDURE generateMockData()
BEGIN
    DECLARE currentTimestamp DATETIME;
    SET currentTimestamp = '2024-02-05 00:00:00';

    WHILE currentTimestamp <= '2024-03-05 05:20:00' DO
        INSERT INTO CryptoCandleHistory (poolId, open, close, high, low, avgPrice, volume, candleTimestamp)
        VALUES (
            UUID(), -- Replace with an actual UUID if needed
            ROUND(0.00022 + (RAND() * (0.005 - 0.00022)), 5),
            ROUND(0.00022 + (RAND() * (0.005 - 0.00022)), 5),
            ROUND(0.00022 + (RAND() * (0.005 - 0.00022)), 5),
            ROUND(0.00022 + (RAND() * (0.005 - 0.00022)), 5),
            ROUND(0.00022 + (RAND() * (0.005 - 0.00022)), 5),
            ROUND(0.00022 + (RAND() * (0.005 - 0.00022)), 5),
            currentTimestamp
        );

        SET currentTimestamp = ADDTIME(currentTimestamp, '00:05:00');
    END WHILE;
END$$

CALL generateMockData();

```

### 使用 DELIMITER

因為 ; 被用在 sql proocedure 內部，所以外部要設置另一個終止符，上面範例為 \$$，然後最後記得在設置回來 `DELIMITER ;`&#x20;

> Inside stored procedures and similar constructs, semicolons are used to end internal statements
