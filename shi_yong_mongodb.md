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
var bodyParser = require('body-parser')
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