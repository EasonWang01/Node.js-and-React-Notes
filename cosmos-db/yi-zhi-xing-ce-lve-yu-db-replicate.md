接著我們看一下 Cosmos DB 的其他功能：

## 1. 調整一致性策略：
在分散式系統中常會發生資料不一致的問題，因為 `Cosmos DB` 預設是多個地區分佈的，所以也會遇到此問題。一般常見的一致性的演算法如：`PBFT`、`Paxos`、`Raft` 等等。

在我們使用 `Apache` 動物園系列服務時會使用 `Zookeeper` 來解決相關一致性問題，而 `Cosmos DB` 內建了幾種一致性策略可以讓我們直接選擇使用。

可以在設定的 `Default consistency` 調整。
![https://ithelp.ithome.com.tw/upload/images/20181103/201124260lqdE7LGk8.png](https://ithelp.ithome.com.tw/upload/images/20181103/201124260lqdE7LGk8.png)

可以參考下圖，查看每個層級的關係：
![https://ithelp.ithome.com.tw/upload/images/20181103/20112426N2rKYTK8Zw.png](https://ithelp.ithome.com.tw/upload/images/20181103/20112426N2rKYTK8Zw.png)

或是在程式內設定一致性策略，覆蓋預設：
```
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Strong
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

或是使用 `session consistency` 的方式，每次讀取或寫入請求時都會重新給予`SessionToken`，讓你重新設定一致性策略。

> 主要為使用 `x-ms-session-token` Header
```
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

## 2. 設定寫入衝突時的解決方案
當有多個地區同時寫入資料時，難免會發生同時寫入的衝突，這時為了要解決誰的資料正確的問題，需要設定相關策略。
![https://ithelp.ithome.com.tw/upload/images/20181103/20112426OPVzePEp5z.png](https://ithelp.ithome.com.tw/upload/images/20181103/20112426OPVzePEp5z.png)
> 預設是採用最後寫入的資料當作依據。

我們也可以設定當發生衝突後使用自訂的 `stored procedure` 解決。
```
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

如果沒有設定由哪個 `stored procedure`（conflictResolutionProcedure）解決的話則會出現在下圖的地方，讓你手動解決。
![https://ithelp.ithome.com.tw/upload/images/20181103/20112426Lv9vKHxIUo.png](https://ithelp.ithome.com.tw/upload/images/20181103/20112426Lv9vKHxIUo.png)

> stored procedure 範例可參考：https://github.com/Azure/azure-cosmosdb-js-server/tree/master/samples/stored-procedures

## 3. 設定 DB replicate 的地區
> 設定越多收費越高。

![https://ithelp.ithome.com.tw/upload/images/20181103/20112426WKqlcxz7eQ.png](https://ithelp.ithome.com.tw/upload/images/20181103/20112426WKqlcxz7eQ.png)

> 有關收費內容可參考：https://azure.microsoft.com/zh-tw/pricing/details/cosmos-db/

## 4. Log 寫入與分析
點選如下介面做設定，可以設定 Log 保留天數與要記錄哪些資訊等等。

![https://ithelp.ithome.com.tw/upload/images/20181103/20112426KAOgHaGWAP.png](https://ithelp.ithome.com.tw/upload/images/20181103/20112426KAOgHaGWAP.png)

https://docs.microsoft.com/zh-tw/azure/cosmos-db/logging

## 5. 監控
選擇 `Metrics`，可以看到一些即時的數據，包含 `Throughput`、`Storage`、`Availability`、`Latency`、`Consistency` 等等。	
![https://ithelp.ithome.com.tw/upload/images/20181103/20112426dXXcOCs2bt.png](https://ithelp.ithome.com.tw/upload/images/20181103/20112426dXXcOCs2bt.png)

也可用發送 API 的方式獲取數據。

```
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider-namespace}/{resource-type}/{resource-name}/metrics?api-version=2014-04-01&$filter={filter}
```

> 發送一個 GET 請求給如上的 Endpoint，可再回傳的 Header 中看到`x-ms-resource-quota` 和 `x-ms-resource-usage`，有關 filter 可參考：https://docs.microsoft.com/en-us/rest/api/monitor/filter-syntax
