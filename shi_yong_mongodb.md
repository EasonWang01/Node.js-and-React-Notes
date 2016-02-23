# 使用MongoDB

使用MongoLab 當練習範例

```
https://mongolab.com/
```

1.先註冊帳號

2.創建資料庫，選地區時記得選擇有免費的

3.創建database內的user

4.創建database內的collection

5.在collection內加入document

##如何測試剛創好的資料庫?

使用Robomongo  https://robomongo.org/

點選Download，選最下面的免費選項下載

1.下載完後開啟，先點選左上方的create 

2.設定一下相關database url、port、username、password(database的，不是你帳戶的)

3.點選左下Test，如成功即可點選save，並進行connect

可參考http://www.codedata.com.tw/database/mongodb-tutorial-1-setting-up-cloud-env/

#如何用Node.js連線到MongoLab?

先使用npm 安裝
```
npm install mongodb
```
```
var mongo = require('mongodb');
var Server = mongo.Server;
var Db = mongo.Db;
```
設定server及資料庫
```
var server = new Server('ds013898.mongolab.com',13898, {auto_reconnect : true});
var db = new Db('forclass', server);

```
設定database中user帳號及密碼(不是MongoLab的登入帳密)
```
db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")
      }else{
      	console.log("client.authenticate error")
      };

    });
});
```
完成後如下，啟動server後如在terminal中出現connect success 即表示成功連線
```
var express = require('express');

////
var mongo = require('mongodb');
var Server = mongo.Server;
var Db = mongo.Db;

var server = new Server('ds013898.mongolab.com',13898, {auto_reconnect : true});
var db = new Db('forclass', server);


db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")
      }else{
      	console.log("client.authenticate error")
      };

    });
});

////
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```
#開始操作資料庫
```
Relation DataBase                         MongoDB
------------------------------------------------------------
資料庫(Database)                           DataBase
資料表(Table)                              Collection
資料(Record/Row)                           Document
欄位(Column)                               Field
主索引(PK)                                 _id
function                                   function ( )
stored procedure                           mapreduce 
```

1.先根據官方範例在MongoLab引入一個範例collection名為apple,而裡面的document如下
```
{
  "address": {
     "building": "1007",
     "coord": [ -73.856077, 40.848447 ],
     "street": "Morris Park Ave",
     "zipcode": "10462"
  },
  "borough": "Bronx",
  "cuisine": "Bakery",
  "grades": [
     { "date": { "$date": 1393804800000 }, "grade": "A", "score": 2 },
     { "date": { "$date": 1378857600000 }, "grade": "A", "score": 6 },
     { "date": { "$date": 1358985600000 }, "grade": "A", "score": 10 },
     { "date": { "$date": 1322006400000 }, "grade": "A", "score": 9 },
     { "date": { "$date": 1299715200000 }, "grade": "B", "score": 14 }
  ],
  "name": "Morris Park Bake Shop",
  "restaurant_id": "30075445"
}
```
接著回到index.js將code改成如下，應可看到query 出整個collection內容
```
var express = require('express');

////
var mongo = require('mongodb');
var Server = mongo.Server;
var Db = mongo.Db;

var server = new Server('ds013898.mongolab.com',13898, {auto_reconnect : true});
var db = new Db('forclass', server);


db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")
       var cursor = db.collection('apple').find();
       
        cursor.each(function(err, doc) {
    
         console.log(doc);
        db.close();
   });
      }else{
      	console.log("client.authenticate error")
      };

    });
});

////
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```
2.如果在find()，裡面放入參數，會query出所有符合的document
```
var cursor = db.collection('apple').find({ "borough": "Bronx" });
       
```

舉例:
假設有個inventory collection裡面有三個collection如下`

```
{ _id: 5, type: "food", item: "aaa", ratings: [ 5, 8, 9 ] }
{ _id: 6, type: "food", item: "bbb", ratings: [ 5, 9 ] }
{ _id: 7, type: "food", item: "ccc", ratings: [ 9, 5, 8 ] }
```
執行 db.inventory.find( { ratings: [ 5, 8, 9 ] } )

將返回
```
{ "_id" : 5, "type" : "food", "item" : "aaa", "ratings" : [ 5, 8, 9 ] }
```
##移除document
```
var cursor = db.collection('apple').remove({});
       
```
##新增document
```
       var cursor = db.collection('apple').insert({
    title: 'web課程', 
    description: 'test ',
    by: 'eason',
    url: 'hi',
    tags: ['hello'],
    likes: 200
});
```