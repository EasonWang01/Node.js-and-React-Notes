# 使用Mysql


臨時免費信箱:http://www.yopmail.com/zh/

測試用免費mysql:https://www.db4free.net/

1.申請好mysql hosting後使用npm安裝mysql
```
npm install mysql --save
```
2.測試連線
```
var express = require('express');

////
var mysql      = require('mysql');

var connection = mysql.createConnection({
  host     : 'db4free.net',
  user     : 'forclass',
  password : 'test123',
  database : 'fruit'
});
connection.connect(function(err) {
  if (err) {
    console.error('error connecting: ' + err.stack);
    return;
  }
 
  console.log('connected as id ' + connection.threadId);
});
////
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```
3.成功連線後可插入資料或到下面網址管理資料庫
https://www.db4free.net/phpMyAdmin

#操控資料庫

##insert TABLE
```
connection.query('CREATE TABLE Food (' +             
                'Food_id int,'+
                'Food_name VARCHAR(100),'+
                'Food_prize VARCHAR(100),'+
                'Food_kind VARCHAR(100),'+
                'PRIMARY KEY(Food_id))', function(err, result){

                    if(err) {
                        console.log(err);
                    }
                    else{
                        console.log("Table Food Created");
                    }
                });
```
記得每段分行要用+號，因為javascript分行要用字串聯接
##insert column
```
connection.query("ALTER table apple add column (code varchar(255))" ,function(err, result) {
    if (err) throw err;

    console.log(result.insertId);
});
```
##insert row
```
connection.query("INSERT INTO Food SET ?",{Food_id:01,Food_name:'Noodle',Food_prize:200,Food_kind:'chinese'
} ,function(err, result) {
    if (err) throw err;
    console.log(result);
    console.log(result.insertId);
});
```
##Read data
```
connection.query('SELECT * FROM Food',function(err,rows){
  if(err) throw err;

  console.log(rows);
});
```
完整版
```
var express = require('express');

////
var mysql      = require('mysql');

var connection = mysql.createConnection({
  host     : 'db4free.net',
  user     : 'forclass',
  password : 'test123',
  database : 'fruit'
});
connection.connect(function(err) {
  if (err) {
    console.error('error connecting: ' + err.stack);
    return;
  }
   console.log('connected as id ' + connection.threadId);

connection.query('SELECT * FROM Food',function(err,rows){
  if(err) throw err;

  console.log(rows);
});

connection.end();
});
////
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```


#使用完記得關閉
```
connection.end();
```


參考至:

https://www.npmjs.com/package/mysql#getting-the-id-of-an-inserted-row

http://www.sitepoint.com/using-node-mysql-javascript-client/