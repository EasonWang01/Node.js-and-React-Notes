# Cosmos DB

Cosmos DB 可以建立 RDBMS 與 NoSQL型態。

而許多人會困惑如果是傳統RDBMS型態，那與原本的Azure SQL區別在哪。

| SQL Server | Cosmos SQL DB |
| :--- | :--- |
| scaling up （垂直擴展） | scaling out（水平擴展） |
| 擴展方式：增加Server CPU, RAM | 擴展方式：新增更多台Server，共同處理請求 |

[https://portal.azure.com/\#create/Microsoft.DocumentDB.3.0.0](https://portal.azure.com/#create/Microsoft.DocumentDB.3.0.0)



有五種 DB 類型可供選擇：![](/assets/Screen Shot 2018-10-31 at 2.16.06 PM.png)

> 這裡我們先嘗試使用 Core \(SQL\)



再來進入服務裡面創建 `collection`

![](/assets/Screen Shot 2018-10-31 at 2.47.52 PM.png)

> 可以看到大致的收費情況

點擊後第二步驟和第三步驟隨即會出現。

![](/assets/Screen Shot 2018-10-31 at 2.54.03 PM.png)

2.下載他第二步驟的的範例 script ，然後用編輯器打開後輸入如下：

```
npm install && npm start
```

之後打開 http://localhost:3000/，隨意輸入一筆資料。

![](/assets/Screen Shot 2018-11-01 at 2.29.15 PM.png)



3.進入DataExplorer

即可看到剛才新增進去的資料

![](/assets/Screen Shot 2018-11-01 at 2.30.48 PM.png)



