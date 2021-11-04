# 進階Mongo

## 1.操作Mongo的Array

[https://docs.mongodb.com/manual/reference/operator/update/pull/#up.\_S\_pull](https://docs.mongodb.com/manual/reference/operator/update/pull/#up.\_S\_pull)

Ex: 以下可把rating\_my Array中的item物件中的\_id與req.body.item.\_id\
值相同的所有欄位移除

```
    User.update({_id: req.token.data._id}, {$pull: {  //先把舊的移除
      rating_my: {
        "item._id": req.body.item._id
      }
    }})
```

## 2.分頁快速query

[http://stackoverflow.com/questions/7228169/slow-pagination-over-tons-of-records-in-mongo](http://stackoverflow.com/questions/7228169/slow-pagination-over-tons-of-records-in-mongo)

## 3.使用Geo search

[https://docs.mongodb.com/manual/reference/operator/query-geospatial/](https://docs.mongodb.com/manual/reference/operator/query-geospatial/)

以下使用$near為範例

1.先在schema建立index

```
var c = new mongoose.Schema({
    status: String,   //物品承租階段狀態 eg,尋租中,已出租,已還租
    rented: Boolean, //是否已出租
    renterInfo: Object,       //承租者相關資訊，填表
    lessee: String,  //承租者
    geometry: {
        type: {
            type: String,
            default: "Point"
        },
        coordinates: [Number]
    }, //物品所在地
    regionName: String,

})

c.index({geometry: '2dsphere'});

exports.Test = mongoose.model('test', c)
```

建立後可以查看到index\


2.之後即可搜尋

```
    Test.find({geometry:
      { $near :
        {
          $geometry: {type : "Point", coordinates: [ -149.0133, 64.7440 ] },
          $minDistance: 0,
          $maxDistance: 50
        }
      }})
    .then(data => {
      //console.log(data)
      res.end(JSON.stringify(data))
    })
    .catch(err => {
      console.log(err);
      res.end(err.toString());
    })
```

3.另外要記得coordinates 是先放Longitude才放Latitude,跟一般google地圖相反

```
$minDistance: 0,
$maxDistance: 50
```

單位為公尺

## 4.新增欄位

新增欄位的意思為在每個document新增一筆資料。

```
db.your_collection.update(
  {},
  { $set: {"new_field": 1} },
  false,
  true
)
```

後面兩個參數分別為：

```
Upsert: If set to true, creates a new document when no document matches the query criteria.

Multi: If set to true, updates multiple documents that meet the query criteria. If set to false, updates one document.
```

[https://stackoverflow.com/a/7714428](https://stackoverflow.com/a/7714428)

## 5.刪除 DB 資料

```
use DB名稱
db.dropDatabase();
```

> 不會刪除 DB內的 USER

## 6. Aggregate

用來進階版 query 資料的方法，可組合出許多進階技。

{% embed url="https://docs.mongodb.com/manual/reference/operator/aggregation" %}

範例： 取得某段時間內的資料累計 (範例資料以小時為單位)

![](<../.gitbook/assets/截圖 2021-11-01 下午2.14.00.png>)

```javascript
const { MongoClient } = require("mongodb");
const marketcap = require("./marketcap-data.js");
// Replace the uri string with your MongoDB deployment's connection string.
const uri = "mongodb://root:example@localhost:27017";
const client = new MongoClient(uri);
async function queryMarketCapData() {
  try {
    await client.connect();
    const database = client.db("...");
    const marketcapCollection = database.collection("market-cap");
    const pipeline = [
      {
        $group: {
          _id: { // 取得月度資料累計
            year: { $year: "$timestamp" },
            //week: { $week: "$timestamp" },
            month: { $month: "$timestamp" },
            //dayOfYear: { $dayOfYear: "$timestamp" },
          },
          value: { $sum: "$value" },
        },
      },
      {
        {
          $sort: { "_id": 1 },
        }
      }
    ];
    const aggCursor = marketcapCollection.aggregate(pipeline);
    for await (const doc of aggCursor) {
      console.log(doc);
    }
  } catch (err) {
    console.log(err);
  }
}

queryMarketCapData();

```

## 三個月為一組資料加總

```javascript
   const pipeline = [
      {
        $group: {
          _id: {
            year: { $year: "$timestamp" },
            month: {
              $subtract: [
                { $month: "$timestamp" },
                { $mod: [{ $month: "$timestamp" }, 3] },
              ],
            },
          },
          value: { $sum: "$value" },
        },
      },
      {
        $sort: { _id: 1 },
      },
    ];
```
