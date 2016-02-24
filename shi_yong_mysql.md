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