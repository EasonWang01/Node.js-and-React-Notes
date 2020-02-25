# 使用 SDK

## SQL API

上一篇稍微看過了 Azure Cosmos DB 的簡單操作後，這一篇會來講解它的大致結構，可參考下圖：

![https://ithelp.ithome.com.tw/upload/images/20181102/20112426pH8epMIjCX.png](https://ithelp.ithome.com.tw/upload/images/20181102/20112426pH8epMIjCX.png)

每個 Azure 帳號可以創建多個 database，每個 database 可以包含多個 collections，在 collections 中包含多個 documents。而在 collection 中可以加入 stored procedures、triggers、User Defined Functions（UDFs） 等等。

我們先手動在 Data Explorer 介面上創建一個 `TestDB` 與 `Fruits` collection。 ![https://ithelp.ithome.com.tw/upload/images/20181102/20112426uC3DoPg0UZ.png](https://ithelp.ithome.com.tw/upload/images/20181102/20112426uC3DoPg0UZ.png)

#### 插入資料到資料庫

接著我們使用程式插入一筆資料試試：

```text
yarn add @azure/cosmos
```

```text
const cosmos = require('@azure/cosmos');
const CosmosClient = cosmos.CosmosClient;

const endpoint = "填上 443 port 的 DB Endpoint";
const masterKey = "填上金鑰";
const client = new CosmosClient({ endpoint, auth: { masterKey } });

const databaseDefinition = { id: 'TestDB' };
const collectionDefinition = { id: 'Fruits' };
const documentDefinition = { name: 'Green Apple', category: 'Apple', data: Date.now() };

async function createFruit() {
  const { body } = await client.database(databaseDefinition.id).container(collectionDefinition.id).items.create(documentDefinition);
  console.log('Created item with content');
}

createFruit().catch(err => {
  console.error(err);
});
```

點進去 Data explorer 的 Document 後可以看到剛才新增的資料如下：

![https://ithelp.ithome.com.tw/upload/images/20181102/20112426IWH0FssgSp.png](https://ithelp.ithome.com.tw/upload/images/20181102/20112426IWH0FssgSp.png)

#### document 結構

接著我們要來講一下每個欄位的意思：

```text
_rid 由系統產生的唯一值，並且有順序性，例如上一筆插入的資料此欄位值為 `SvJDANLJWUiBhB4AAAAAAA==` 則下一筆會是 `SvJDANLJWUiChB4AAAAAAA==`

_etag 由系統產生， 用來優化 concurrency

_ts 由系統產生，當作最後一次更新的 timestamp

_self 由系統產生，代表資源的一個 URI，通常為此種格式 `/dbs/{dbName}/users/{userId}/permissions/{permissionId}`

id 可以由使用者自行設定，如果沒有設定系統會自動產生。
```

#### 我們使用 Node.js SDK 時可以如下使用：

初始化連線：

```text
const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
```

創建資料庫：

```text
const { database } = await client.databases.createIfNotExists({ id: databaseId });
```

建立 collection：

```text
const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
```

執行 SQL

```text
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

參考資料：[https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-resources](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-resources)

## Cassandra API

### Cassandra DB 簡介

相信已經許多人先前已經搭配 `Spark` 使用過，不過因為本賽季後續會使用，所以還是簡介一下。Cassandra 本身是 2008 由 `Facebook` 開源的一個 NoSQL DB，轉眼間已經十年，與 IT 鐵人賽一樣。Cassandra 的資料是以 `Key-Value` 的方式儲存，資料模型與 `Google BigTable` 和 `HBase` 類似，但 Cassandra 在建立 `cluster` 中不會有 `master` 節點，每個節點都是平等的，這樣可以避免單一節點有問題時造成整個系統故障。

![https://ithelp.ithome.com.tw/upload/images/20181105/20112426dYUQHlvSly.png](https://ithelp.ithome.com.tw/upload/images/20181105/20112426dYUQHlvSly.png)

Cassandra 的查詢語言 `CQL` 與 `SQL` 類似，可參考官方文件：[https://cassandra.apache.org/doc/latest/cql/index.html](https://cassandra.apache.org/doc/latest/cql/index.html)

使用 Cassandra 一開始要先建立一個 `KEYSPACE`，同時設定 replication 的方式。

```text
CREATE  KEYSPACE [IF NOT EXISTS] keyspace_name 
   WITH REPLICATION = { 
      'class' : 'SimpleStrategy', 'replication_factor' : N } 
     | 'class' : 'NetworkTopologyStrategy', 
       'dc1_name' : N [, ...] 
   }
   [AND DURABLE_WRITES =  true|false] ;
```

> replication 策略包含以下兩種： 1.`SimpleStrategy` 只有一個`datacenters` 和一個 `rack` 時使用，通常用作測試環境，會設定 `replication_factor` 代表同步儲存資料的節點數目。 2.`NetworkTopologyStrategy` 通常用在生產正式環境，設定 datacenters 的各個名稱，與分別要同步的節點數目。 [https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeReplication.html](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeReplication.html)

相關名詞介紹：[https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html)

### CAP

`SaaS` 有 [twelve-factor](https://12factor.net/)，分散式系統有 `CAP`，`Cassandra` 屬於比較偏向 `AP` 的部分。

熟悉的圖： ![https://ithelp.ithome.com.tw/upload/images/20181105/20112426Zpk7ZaDgog.png](https://ithelp.ithome.com.tw/upload/images/20181105/20112426Zpk7ZaDgog.png)

> 來源：[http://digbigdata.com/wp-content/uploads/2013/05/media\_httpfarm5static\_mevIk.png](http://digbigdata.com/wp-content/uploads/2013/05/media_httpfarm5static_mevIk.png)

### Azure Cosmos DB 的 `Cassandra API`

`Azure Cosmos DB` 的 `Cassandra API` 與 [Apache Cassandra](https://cassandra.apache.org/) 可以無縫的整合，因為使用相同的 `CQLv4` （ Cassandra Query Language），只需要改一下連線的字串，即可將原本連線到自行架設的 `Cassandra DB` 轉為連線到 `Azure Cosmos DB`。

無痛轉移到 `Azure Cosmos DB` 的 `Cassandra` ，以後再也不用去監控 DB 機器的狀態，以及 CPU、 RAM 使用量突然飆高等問題，也不用管理一堆 `yaml` 等等的配置檔案以及煩惱配置跨地區 `cluster` 與設定網路等等，通通都交給 `Azure Cosmos DB` 幫你處理即可。`throughput` 與 `latency` 出問題時再也不用 `Dear DevOps` 然後 `Dear SysOps`，只要於 `portal` 面板按個按鈕即可解決。

下一篇將介紹如何實際使用 Azure Cosmos DB 的 `Cassandra API` 。

這篇文章要來實際使用一下 `Azure Cosmos DB` 的 `Cassandra API`。

如果已經安裝了 [`cqlsh`](http://cassandra.apache.org/doc/4.0/tools/cqlsh.html) 可以直接用如下方式連線：

```text
set SSL_VERSION=TLSv1_2
set SSL_VALIDATE=false

cqlsh yichengcosmos.cassandra.cosmosdb.azure.com 10350 -u yichengcosmos -p <填上 PRIMARY PASSWORD> --ssl
```

### 使用官方範例

首先我們一樣先使用看看官方提供的範例：

```text
git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-nodejs-getting-started.git && cd  azure-cosmos-db-cassandra-nodejs-getting-started && npm install
```

然後到 portal 把 `Connection string` 記錄下來。 ![https://ithelp.ithome.com.tw/upload/images/20181106/20112426bWuPeN3hez.png](https://ithelp.ithome.com.tw/upload/images/20181106/20112426bWuPeN3hez.png)

填入 `config.js` 中 ![https://ithelp.ithome.com.tw/upload/images/20181106/20112426axdF7DYoJy.png](https://ithelp.ithome.com.tw/upload/images/20181106/20112426axdF7DYoJy.png)

> `config.contactPoint` 記得在後面加上 PORT 號

#### 產生憑證

因為在連線需要填入 `sslOptions` 所以要一張憑證，首先在此處官方提供的連結下載憑證：[https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt)

然後把他移動到專案目錄下，並把檔案副檔名改為 `cer`。

```text
mkdir ./cert && mv ~/Downloads/bc2025.crt ./cert/bc2025.cer
```

最後更改一下 `uprofile.js` 的 `cert` 路徑。 ![https://ithelp.ithome.com.tw/upload/images/20181106/2011242615yKR52M8v.png](https://ithelp.ithome.com.tw/upload/images/20181106/2011242615yKR52M8v.png)

之後執行程式： `node uprofile.js`。

> 這時各位可能會產生 `Error: error:0906D06C:PEM routines:PEM_read_bio:no start line` 的錯誤，然後十分困惑。

不過不要緊，我們不要用官方的，自己來產生一張 `X509` 證書即可：

```text
openssl req -newkey rsa:2048 -new -nodes -keyout key.pem -out csr.pem
```

輸入後填上相關資訊，也可以都按 Enter

然後從 `pem` 轉為 `crt`

```text
openssl x509 -req -days 365 -in csr.pem -signkey key.pem -out server.crt
```

最後再把 `server.crt` 改名字為 `server.cer` 即可

```text
mv server.crt server.cer
```

![https://ithelp.ithome.com.tw/upload/images/20181106/2011242670eI4XZNJV.png](https://ithelp.ithome.com.tw/upload/images/20181106/2011242670eI4XZNJV.png)

之後執行範例程式：

```text
node uprofile.js
```

> 這時會出現 `{ TypeError: Cannot read property 'type' of undefined` 的錯誤，因為 Azure 目前的官方範例有點問題，在 `batch` 批量 `Insert` 的時候 `params` 多了一個參數。

這時把最後面的日期參數移除後再次執行：

> 沒想到又出現如下錯誤： `{ ResponseError: Logged batches are not supported by the service yet. Please use unlogged batches instead.`
>
> `logged batches` 代表該 batch 執行是原子性的，而 `cosmos DB` 目前不支援，只能用 `unlogged batched`。但是 `cosmos DB` 通常會有多個 `partition` 所以如果使用 `unlogged batched` 為 `anti-pattern` 的操作，可參考：[https://stackoverflow.com/a/49471102](https://stackoverflow.com/a/49471102) 在 Azure 解決這個範例錯誤與 batch 的問題之前我們先將它改為一般的 `Insert`。

### 範例：

我們接著不使用官方範例，把 `uprofile.js` 直接改為以下，自己來操作看看。

#### 創建 DB（Keyspace）：

```text
const cassandra = require('cassandra-driver');
const tls = require("tls");
const fs = require("fs");

const config = require("./config");

const ssl_option = {
  cert: fs.readFileSync("./cert/server.cer"),
  secureProtocol: "TLSv1_2_method"
};

const authProviderLocalCassandra = 
    new cassandra.auth.PlainTextAuthProvider(config.username, config.password);
const client = new cassandra.Client({ 
  contactPoints: [config.contactPoint], 
  authProvider: authProviderLocalCassandra, 
  sslOptions:ssl_option
});

client.connect();

const query = "CREATE KEYSPACE IF NOT EXISTS Fruits WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter\' : \'1\' }";

client.execute(query)
  .then(result => {
    console.log(result);
    client.shutdown();
  })
  .catch(err => console.log(err));
```

#### 創建 TABLE

```text
const cassandra = require("cassandra-driver");
const tls = require("tls");
const fs = require("fs");

const config = require("./config");

const ssl_option = {
  cert: fs.readFileSync("./cert/server.cer"),
  secureProtocol: "TLSv1_2_method"
};

const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(
  config.username,
  config.password
);
const client = new cassandra.Client({
  contactPoints: [config.contactPoint],
  authProvider: authProviderLocalCassandra,
  sslOptions: ssl_option
});

client.connect()
  .then(function () {
    const query = "CREATE TABLE IF NOT EXISTS Fruits.banana " +
      "(_id int PRIMARY KEY, price int, name text)";
    return client.execute(query);
  })
  .catch(function (err) {
    console.error('There was an error', err);
    return client.shutdown();
  });
```

#### 插入資料與查詢資料：

```text
const cassandra = require("cassandra-driver");
const tls = require("tls");
const fs = require("fs");

const config = require("./config");

const ssl_option = {
  cert: fs.readFileSync("./cert/server.cer"),
  secureProtocol: "TLSv1_2_method"
};

const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(
  config.username,
  config.password
);
const client = new cassandra.Client({
  contactPoints: [config.contactPoint],
  authProvider: authProviderLocalCassandra,
  sslOptions: ssl_option
});

client.connect()
  .then(function () {
    console.log('Inserting');
    const query = 'INSERT INTO Fruits.banana (_id, price, name) VALUES (?, ?, ?)';
    return client.execute(query, [1, 100, "Super banana"], { prepare: true});
  })
  .then(function () {
    const query = 'SELECT price, name FROM Fruits.banana WHERE _id = ?';
    return client.execute(query, [1], { prepare: true });
  })
  .then(function (result) {
    const row = result.first();
    console.log('Retrieved row: %j', row);
    return client.shutdown();
  })
  .catch(function (err) {
    console.error('There was an error', err);
    return client.shutdown();
  });
```

回到 `portal` 的 `Data explorer` 即可看到剛才插入的資料： ![https://ithelp.ithome.com.tw/upload/images/20181106/20112426Cui9CdThg5.png](https://ithelp.ithome.com.tw/upload/images/20181106/20112426Cui9CdThg5.png)

> 有時插入資料後按下 portal 頁面上的重新整理不會有反應，要把網頁重新整理一次才行。

那我們這篇介紹就到這邊為止，有興趣的朋友可以稍微玩一下。

