# 簡單範例

延續前兩章

現在專案目錄如下
```

 index.js

 package.json

|| node_modules

|| public

|| routes----index.js
```
    
根目錄的index.js為
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
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```
routes下的index.js為
```
module.exports = function (app) {

app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Helloo worldd');
 
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};
```
public資料夾為空

#使用Handlebars 當模板引擎
```
npm install express-handlebars
```
在根目錄的index.js加入
```

var exphbs  = require('express-handlebars');

```
之後再下面再加入(記得加在var app = express();後)
```

app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');
```
再把routes中的index.html裡面的這段改為
```
 app.get('/', function (req, res) {
    res.render('main');///從send改為render
 
  });

```
最後創建views資料夾於根目錄，裡面放入main.handlebars
```
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>
    <h1>title</h1>
    <p>Welcome to Handlebars</p>
  </body>
</html>
```
資料夾名稱必須為views，因為那是express預設的模板資料夾

重啟server即可看到畫面。

##之後我們試著讓render時傳入參數
routes內的index.js改為
```
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm Title"});
 
  });
```
views內的main.handlebars改為
```
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>
    <h1>{{title}}</h1>
    <p>Welcome to Handlebars</p>
  </body>
</html>
```
即可看到，產生的畫面資料變為render提供的第二個物件參數的值

##使用簡單的條件判斷render

先在routes的index.js
```
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm fhTitle",title1:false});
 
  });
 
```
main.handlebars
```
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>

  {{#if title1}}
    <h1>It's true</h1>
  {{else}}
    <h1>It's false</h1>
  {{/if}}

    <h1>{{title}}</h1>
   
    <p>Welcome to Handlebars</p>
  </body>
</html>
```
之在在把
```
res.render('main',{title:"I'm fhTitle",title1:false});
```
title1的false改為true看看

##使用each再render時 去遍歷陣列

```
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm fhTitle", people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]});
```
```
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>
 {{#each people}}
    <li>{{this}}</li>
  {{/each}}

    <h1>{{title}}</h1>
   
    <p>Welcome to Handlebars</p>
  </body>
</html>
```