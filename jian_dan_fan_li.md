# 簡單範例

延續前兩章

現在專案目錄如下

 index.js

 package.json

|| node_modules

|| public

|| routes----index.js
    
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

#使用Handlebars