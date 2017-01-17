# 使用MongoDB

```
Mongo資料庫 > collection >document

SQL資料庫   >   資料表   > 資料列
```

使用MongoLab 當練習範例

!!注意，MongoLab於2016二月底改名為mLab，連結的資料庫路徑也改了

```
https://mlab.com/home
```

1.先註冊帳號

2.創建資料庫，選地區時記得選擇有免費的

3.創建database內的user

4.創建database內的collection

5.在collection內加入document

## 如何測試剛創好的資料庫?

使用Robomongo  [https://robomongo.org/](https://robomongo.org/)

點選Download，選最下面的免費選項下載

1.下載完後開啟，先點選左上方的create

2.設定一下相關database url、port、username、password\(database的，不是你帳戶的\)

3.點選左下Test，如成功即可點選save，並進行connect

可參考[http://www.codedata.com.tw/database/mongodb-tutorial-1-setting-up-cloud-env/](http://www.codedata.com.tw/database/mongodb-tutorial-1-setting-up-cloud-env/)

# 如何用Node.js連線到MongoLab?

> 注意：使用mLab註冊帳號後要去新增使用者，之後連線的url中的dbuser是你之後新增的使用者名稱跟密碼，不是輸入帳號

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
var server = new Server('ds013898.mlab.com',13898, {auto_reconnect : true});
var db = new Db('forclass', server);
```

填入database中user帳號及密碼\(不是MongoLab的登入帳密\)

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

var server = new Server('ds013898.mlab.com',13898, {auto_reconnect : true});
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





app.listen(8080);
```

# 開始操作資料庫

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

var server = new Server('ds013898.mlab.com',13898, {auto_reconnect : true});
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

2.如果在find\(\)，裡面放入參數，會query出所有符合的document

```
var cursor = db.collection('apple').find({ "borough": "Bronx" });
```

舉例:  
假設有個inventory collection裡面有三個collection如下\`

```
{ _id: 5, type: "food", item: "aaa", ratings: [ 5, 8, 9 ] }
{ _id: 6, type: "food", item: "bbb", ratings: [ 5, 9 ] }
{ _id: 7, type: "food", item: "ccc", ratings: [ 9, 5, 8 ] }
```

執行 db.inventory.find\( { ratings: \[ 5, 8, 9 \] } \)

將返回

```
{ "_id" : 5, "type" : "food", "item" : "aaa", "ratings" : [ 5, 8, 9 ] }
```

## 移除document

```
var cursor = db.collection('apple').remove({});
```

## 新增document

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

## 運用operator

以下參考至[http://www.runoob.com/mongodb/mongodb-operators.html](http://www.runoob.com/mongodb/mongodb-operators.html)

先輸入三組document

```
db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")

    db.collection('apple').insert({
    title: 'web課程', 
    description: 'test ',
    by: 'eason',
    url: 'hi',
    tags: ['hello'],
    likes: 50
});
        db.collection('apple').insert({
    title: 'web課程', 
    description: 'test ',
    by: 'eason',
    url: ['hi'],
    tags: 'hello',
    likes: 100
});
         db.collection('apple').insert({
    title: 'web課程', 
    description: 'test ',
    by: 'eason',
    url: true,
    tags: ['hello'],
    likes: 200
});

      }else{
          console.log("client.authenticate error")
      };

    });
});
```

接著改成查詢的code如下

```
db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")
       var cursor = db.collection('apple').find({likes : {$gt : 100}});

        cursor.each(function(err, doc) {

         console.log(doc);
     db.close();
   });
      }else{
          console.log("client.authenticate error")
      };

    });
});
```

# $gt代表大於

# $lt代表小於

# $gte大於等於

# $lte小於等於

---

## 使用type操作

```
db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")
       var cursor = db.collection('apple').find({url : {$type : 8}});

        cursor.each(function(err, doc) {

         console.log(doc);
     db.close();
   });
      }else{
          console.log("client.authenticate error")
      };

    });
});
```

type的值，數字對照表

```
Double    1     
String    2     
Object    3     
Array    4     
Binary data    5     
Object id    7     
Boolean    8     
Date    9     
Null    10     
Regular Expression    11     
JavaScript    13     
Symbol    14     
JavaScript (with scope)    15     
32-bit integer    16     
Timestamp    17     
64-bit integer    18
```

# 使用limit\(\)

如果為參數1代表讀到一個document，如果為五代表讀前五個document

```
var cursor = db.collection('apple').find().limit(1);
```

# 使用skip\(\)

與limit相反，跳過skip參數個document，都是從前面往後數

```
var cursor = db.collection('apple').find().skip(2);
```

# 使用sort\(\)

根據name的值去排列，而不是根據document的index順序

```
var cursor = db.collection('apple').find().sort({"likes":-1})
```

# 比較這兩個Find\(\)

```
var cursor = db.collection('apple').find({},{likes:1, _id: 0});
var cursor = db.collection('apple').find({likes : {$gt : 100}},{likes:1, _id: 0});
```

發現find\(\)的第一個參數代表:我們要從哪個地方去找東西

第二個參數代表:從那個地方要找那些東西出來

第二個參數中物件的值只有0和1，指定其他數和指定1的效果相同，0為不顯示

參考至:

中文:

[http://calvert.logdown.com/posts/159792-sql-to-mongodb-mapping-chart](http://calvert.logdown.com/posts/159792-sql-to-mongodb-mapping-chart)

英文:官方doc:

[https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/\#db.collection.createIndex](https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#db.collection.createIndex)

英文:官方github.io:

[http://mongodb.github.io/node-mongodb-native/2.0/api/Collection.html\#findOne](http://mongodb.github.io/node-mongodb-native/2.0/api/Collection.html#findOne)

## 如果是下載到local端啟用

```
1.先到你的Mongo資料庫下bin的外面創建資料夾
   2.cd到bin裡面把路徑複製
   3.使用admin開起cmd在cd到剛複製的路徑
   4.執行mongod --dbpath ../資料夾名稱/
   5.即可使用robomongo連線
```

1.建造database

```
use 資料庫名稱  //如不存在即會創建新的
```

ps:用Robomongo執行以上指令如發現沒出現，需要點選新連線，才會出現

2.建造collection

```
db.createCollection("apple")
```

# Node.js 連接

```
var MongoClient = require('mongodb').MongoClient
  , assert = require('assert');

// Connection URL
var url = 'mongodb://localhost:27017/myproject';
// Use connect method to connect to the Server
MongoClient.connect(url, function(err, db) {
  assert.equal(null, err);
  console.log("Connected correctly to server");

  db.close();
});
```

# Mongo有新增的連接方法，參考下面文章

主要是寫關於require\('mongodb'\).Db和require\('mongodb'\).MongoClient的區別  
\(其告知MongoClient為較新的方法，推薦使用\)

[http://mongodb.github.io/node-mongodb-native/driver-articles/mongoclient.html](http://mongodb.github.io/node-mongodb-native/driver-articles/mongoclient.html)

# 使用Mongoose

介紹

```
Schema  ：  描述數據結構

Model   ：  由Schema生成的模型

Entity  ：  由Model創建的實體
```

開始使用

1.

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://user:pass@host:port/dbs');
```

\(可點選mLab的tools標籤，看相關連線資料\)  
如何抓取連線時的錯誤

```
mongoose.connect('mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass',function(err){
    if(err){throw err};
});
```

如何抓取正確連線到資料庫的訊息

```
db.once('open', function() {
  console.log("connect mongo")
});
```

如何抓取連線後執行時的錯誤

```
db.on('error', console.error.bind(console, 'connection error:'));
```

完整

```
var mongoose = require("mongoose");
mongoose.connect('mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass',function(err){
    if(err){throw err};
});
var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log("connect mongo")
});
```

\(存入資料時如collection名稱不存在則會自動建立\)

2.

定義model\(這裡省略先定義schema，直接定義在MODEL內\)

第一個參數為collection的名稱

```
var Cat = mongoose.model('Cat', {
  name: String,
  friends: [String],
  age: Number,
});
```

3.存入資料\(產生實體\)

```
var kitty = new Cat({ name: 'Zildjian', friends: ['tom', 'jerry']});
kitty.age = 3;
```

4.使用save才真的存入

```
kitty.save(function (err) {
  if (err) // ...
  console.log('meow');
});
```

ps:如果存入資料的欄位不在schema內則不會存入

ps:如省略某些欄位沒寫，則不會顯示，亦可正常存入

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass');

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log("connect");
});
var Cat = mongoose.model('Cat', {
  name: String,
  friends: [String],
  age: Number,
});


var kitty = new Cat({ name: 'Zildjian', friends: ['tom', 'jerry']});
kitty.age = 3;

//使用save方法後才會存入
kitty.save(function (err) {
  if (err) // ...
  console.log('meow');
});
```

# 使用Promise

```
var User = mongoose.model('ac', new mongoose.Schema({
    name:{type: String, unique: true},
    password:String
}));
var list = new User({name:"s", password:"123456"});
list.save()
.then(a => console.log(a))
.catch(err => console.log(err));
```

## 注意：記得res.end回傳資料要先`JSON.stringify`

# 查詢資料

```
var Cat = mongoose.model('Cat', {
  name: String,
  friends: [String],
  age: Number,
});
Cat.find({},function(err,doc){

    console.log(doc);
});
```

得知必須使用先前定義好Model才能查找

但如果改成下面呢?

```
var Cat = mongoose.model('Cat', {});
Cat.find({},function(err,doc){

    console.log(doc);
});
```

發現一樣可查找，而上面的例子我們將Schema留空，於是我們知道，可以只提供collection的參數即可，後面Schema參數如果不寫會報錯，但其可接受空的物件當參數。

### 其他查找方法和原生相似

```
Cat.find({},{_id:1},function(err,doc){

    console.log(doc);
});
```

find\(\)

1.第一個參數為要搜尋哪些document

2.第二個參數為要顯示document內的那些資料\(1代表要，0代表不要\)

3.第三個參數為一個function\(err,doc\)  
，讀取到的資料會顯示在doc這

```
var find = Cat.find({},{time:1,_id:0},function(err,doc){
res.render("home",{text:doc});

});
```

## 我們也可先定義Schema在把他compile到model內

```
var kittySchema = mongoose.Schema({
    name: String
});

var Kitten = mongoose.model('cats', kittySchema);
```

這樣和上面直接將schema在model內定義是相同的，不同之處在於有了例外定義的Schema，我們可以幫Schema指定方法

但記得使用methods函式指定方法的話，要放在model實例化之前

```
kittySchema.methods.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}
```

完整版

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass');

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log("connect");
});
var kittySchema = mongoose.Schema({
    name: String
});

kittySchema.methods.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}
//////////method要定義在model實例化之前
var Kitten = mongoose.model('cats', kittySchema);

var fluffy = new Kitten({ name: 'fluffy' });
fluffy.speak(); // "Meow name is fluffy"
```

## 有另外一個幫Schema指定方法的方式嗎?

使用static

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass');

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log("connect");
});
var kittySchema = mongoose.Schema({
    name: String
});

kittySchema.statics.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}

var Kitten = mongoose.model('cats', kittySchema);

Kitten.speak();
```

發現兩種的區別為

```
methods:為定義好後給Entity使用的方法

statics:為定義好後給model使用的方法


ps:(字尾記得+S)
```

# 刪除document

```
var Cat = mongoose.model('Dog', {
  name: String,
  time: String
});

Cat.remove({},function(err){
    if(err){throw err};
});
```

# 更新document

\(參數一定要放入callback否則不會update\)

```
Cat.update({_id:req.body.id[0]},{time:req.body.event[0],name:req.body.event[1]},function(err){
        if(err){
        console.log(err);
         }
    });
```

第二個參數前可放入運作子  
[https://docs.mongodb.org/manual/reference/operator/update/](https://docs.mongodb.org/manual/reference/operator/update/)

---

參考至:  
[http://mongoosejs.com/docs/2.7.x/docs/methods-statics.html](http://mongoosejs.com/docs/2.7.x/docs/methods-statics.html)

[https://cnodejs.org/topic/504b4924e2b84515770103dd](https://cnodejs.org/topic/504b4924e2b84515770103dd)

# 注意事項

1.

在schema中定義Date如果如下兩種

```
PostDate: String,
lastModify: Date,
```

node.js

```
PostDate: Date.now() + 1000 * 60 * 60 * 8, //因此預設為UTC+0 要改為UTC+8
lastModify: Date.now() + 1000 * 60 * 60 * 8,
```

則String型態會存成

```
148043043057
```

Date會存成

```
2016-11-29T12:15:03.713Z
```

也就是設為Date 的schema會自動轉為ISO

---

1. 在Schema寫好後如果沒有存入資料則該欄會為空值

```
comments: Array,

    如上的schema如在後端沒指定值則comments這欄為[]
```

如果想要在已有的collection新增field可使用update配合$set即可



避免使用update 中的 upsert 會產生\_id為null的情況

