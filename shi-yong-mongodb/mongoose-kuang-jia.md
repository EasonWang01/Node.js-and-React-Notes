---
description: 框架
---

# Mongoose 框架

為 mongoDB 的一個框架。

## 使用Mongoose

> mongoose query 的物件沒辦法直接修改，要用 lean() 後才可修改，且 lean 可增加許多 query 效能

[https://stackoverflow.com/a/68553745/4622645](https://stackoverflow.com/a/68553745/4622645)

> 如出現連線字串後加入/\<db name> 出現 auth error 請參考以下模板連線方式：
>
> [https://github.com/EasonWang01/Nodejs-server-API-boilerplate](https://github.com/EasonWang01/Nodejs-server-API-boilerplate)

介紹

```
Schema  ：  描述數據結構

Model   ：  由Schema生成的模型

Entity  ：  由Model創建的實體
```

開始使用

1\.

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://user:pass@host:port/dbs');
```

(可點選mLab的tools標籤，看相關連線資料)\
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

(存入資料時如collection名稱不存在則會自動建立)

2\.

定義model(這裡省略先定義schema，直接定義在MODEL內)

第一個參數為collection的名稱

```
var Cat = mongoose.model('Cat', {
  name: String,
  friends: [String],
  age: Number,
});
```

3.存入資料(產生實體)

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

ps: 如果存入資料的欄位不在schema內則不會存入

ps: 如省略某些欄位沒寫，則不會顯示，亦可正常存入

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

## 使用Promise

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

### 注意：記得res.end回傳資料要先`JSON.stringify`

## 查詢資料

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

#### 其他查找方法和原生相似

```
Cat.find({},{_id:1},function(err,doc){

    console.log(doc);
});
```

find()

1.第一個參數為要搜尋哪些document

2.第二個參數為要顯示document內的那些資料(1代表要，0代表不要)

3.第三個參數為一個function(err,doc)\
，讀取到的資料會顯示在doc這

```
var find = Cat.find({},{time:1,_id:0},function(err,doc){
res.render("home",{text:doc});

});
```

### 我們也可先定義Schema在把他compile到model內

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
