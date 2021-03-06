# Cosmos DB

## Cosmos DB

Cosmos DB 在 collection 的資料都是以 JSON 格式儲存。

| SQL Server | Cosmos SQL DB |
| :--- | :--- |
| scaling up （垂直擴展） | scaling out（水平擴展） |
| 擴展方式：增加Server CPU, RAM | 擴展方式：新增更多台Server，共同處理請求 |

[https://portal.azure.com/\#create/Microsoft.DocumentDB.3.0.0](https://portal.azure.com/#create/Microsoft.DocumentDB.3.0.0)

有五種 DB 類型可供選擇：![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-10-31%20at%202.16.06%20PM.png)

> 這裡我們先使用 Core \(SQL\)

再來進入服務裡面創建 `collection`

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-10-31%20at%202.47.52%20PM.png)

> 可以看到大致的收費情況

點擊後第二步驟和第三步驟隨即會出現。

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-10-31%20at%202.54.03%20PM.png)

2.下載他第二步驟的的範例 script ，然後用編輯器打開後輸入如下：

```text
npm install && npm start
```

之後打開 [http://localhost:3000/，隨意輸入一筆資料。](http://localhost:3000/，隨意輸入一筆資料。)

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-11-01%20at%202.29.15%20PM.png)

3.進入DataExplorer

即可看到剛才新增進去的資料

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-11-01%20at%202.30.48%20PM.png)

## 分析程式

在 config.js 內可以看到連線相關資訊。

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-11-01%20at%203.31.38%20PM.png)

接著可以使用此官方SDK：

[https://github.com/Azure/azure-cosmos-js\#readme](https://github.com/Azure/azure-cosmos-js#readme)

## 使用方式

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/Screen%20Shot%202018-11-01%20at%204.15.18%20PM.png)

執行 SQL Query:

```javascript
const querySpec = {
    query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
    parameters: [
        {
            name: "@lastName",
            value: "Andersen"
        }
    ]
};

const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
for (var queryResult of results) {
    let resultString = JSON.stringify(queryResult);
    console.log(`\tQuery returned ${resultString}\n`);
}
```

[https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-nodejs\#review-the-code](https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-nodejs#review-the-code)

## 使用 Rest API

[https://docs.microsoft.com/en-us/rest/api/cosmos-db/list-documents](https://docs.microsoft.com/en-us/rest/api/cosmos-db/list-documents)

## Node.js SDK 文件

[https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-nodejs-samples](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-nodejs-samples)

