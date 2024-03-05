# function

可用 function 可以進行複雜的條件 Query:

[https://sequelize.org/docs/v6/core-concepts/model-querying-basics/#advanced-queries-with-functions-not-just-columns](https://sequelize.org/docs/v6/core-concepts/model-querying-basics/#advanced-queries-with-functions-not-just-columns)

範例：

> 以下為 query by interval, 例如交易圖表 api，可以讓使用者選時間段，給不同的 candlebar 資料

```javascript
 const getIntervalCondition = (interval) => {
      switch (interval) {
        case "5m":
          return sequelize.where(
            sequelize.fn(
              "MOD",
              sequelize.fn("minute", sequelize.col("candleTimestamp")),
              5
            ),
            0
          );
        case "15m":
          return sequelize.where(
            sequelize.fn(
              "MOD",
              sequelize.fn("minute", sequelize.col("candleTimestamp")),
              15
            ),
            0
          );
        case "30m":
          return sequelize.where(
            sequelize.fn(
              "MOD",
              sequelize.fn("minute", sequelize.col("candleTimestamp")),
              30
            ),
            0
          );
        case "1h":
          return sequelize.where(
            sequelize.fn("minute", sequelize.col("candleTimestamp")),
            0
          );
        case "4h":
          return {
            [Op.and]: [
              sequelize.where(
                sequelize.fn(
                  "MOD",
                  sequelize.fn("HOUR", sequelize.col("candleTimestamp")),
                  4
                ),
                0
              ),
              sequelize.where(
                sequelize.fn("MINUTE", sequelize.col("candleTimestamp")),
                0
              ),
            ],
          };
        case "1d":
          return sequelize.where(
            sequelize.fn("hour", sequelize.col("candleTimestamp")),
            0
          );
        case "1w":
          return {
            [Op.and]: [
              sequelize.where(
                sequelize.fn("DAYOFWEEK", sequelize.col("candleTimestamp")),
                2 // 2 represents Monday in MySQL
              ),
              sequelize.where(
                sequelize.fn("HOUR", sequelize.col("candleTimestamp")),
                0
              ),
              sequelize.where(
                sequelize.fn("MINUTE", sequelize.col("candleTimestamp")),
                0
              ),
            ],
          };
        default:
          throw new Error("Invalid interval");
      }
    };
    const whereCondition = {
      poolId: poolId,
      [Op.and]: getIntervalCondition(interval),
    };
    const candleHistoryRecords = await CryptoCandleHistory.findAll({
      where: whereCondition,
      order: [["candleTimestamp", "ASC"]],
      attributes: [
        [sequelize.literal("UNIX_TIMESTAMP(candleTimestamp)"), "time"],
        "candleTimestamp",
        "open",
        "high",
        "low",
        "close",
        "volume",
      ],
    });
```
