#使用MYSQL


臨時免費信箱:http://www.yopmail.com/zh/

測試用免費mysql:https://www.db4free.net/signup.php
(預設一個資料庫，不可再增加或修改)

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

app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```
3.成功連線後可插入資料或到下面網址管理資料庫
https://www.db4free.net/phpMyAdmin

#操控資料庫

##CREATE TABLE
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

###或是

使用ES6 的新字元串` ` `，可跨行連接字段
```
var a = (`CREATE TABLE articles (
  id     INT PRIMARY KEY AUTO_INCREMENT,
  author VARCHAR(100) NOT NULL,
  title  VARCHAR(100) NOT NULL,
  body   TEXT         NOT NULL
)`);

var query = connection.query(a, function (err, result) {
  if (err) {
    console.error(err);
    return;
  }
  console.error(result);
});

```
如要使用變數記得用`${}`


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
另一種方式

```

//留著當Schema
var a = (`CREATE TABLE articles (
  id     INT PRIMARY KEY AUTO_INCREMENT,
  author VARCHAR(100) NOT NULL,
  title  VARCHAR(100) NOT NULL,
  body   TEXT         NOT NULL
)`);


var article = {
  author: 'eason',
  title: 'HI',
  body: 'this'
};

var query = connection.query('insert into articles set ?', article, function (err, result) {
  if (err) {
    console.error(err);
    return;
  }
  console.error(result);
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
或是使用`??`
```

connection.query('SELECT * FROM ??',"articles",function(err,rows){
  if(err) throw err;

  console.log(rows);
});
```
#UPDATE
```

var query = connection.query("UPDATE articles SET title = ? ","Hello M",function (err, result) {
  if (err) {
    console.error(err);
    return;
  }
  console.error(result);
});

```
加上where，限制要更新的row
```

var query = connection.query("UPDATE articles SET title = ? WHERE id = 1","Hello MyS",function (err, result) {
  if (err) {
    console.error(err);
    return;
  }
  console.error(result);
});
```

使用?號
```


var query = connection.query("UPDATE articles SET title = ? WHERE id = ?",["Hell",1],function (err, result) {
  if (err) {
    console.error(err);
    return;
  }
  console.error(result);
});

```
##有關? 與??

```
指定name時用`??`
指定value用`?`
```
範例1
```
var sql = "SELECT * FROM ?? WHERE ?? = ?";
```
範例2
```
connection.query("UPDATE ?? SET title = ? WHERE id = ?",["articles","Hell",1]
```
#使用escape，防止injection
可達到的效果
```
Numbers are left untouched
Booleans are converted to true / false
Date objects are converted to 'YYYY-mm-dd HH:ii:ss' strings
Buffers are converted to hex strings, e.g. X'0fa5'
Strings are safely escaped
Arrays are turned into list, e.g. ['a', 'b'] turns into 'a', 'b'
Nested arrays are turned into grouped lists (for bulk inserts), e.g. [['a', 'b'], ['c', 'd']] turns into ('a', 'b'), ('c', 'd')
Objects are turned into key = 'val' pairs for each enumerable property on the object. If the property's value is a function, it is skipped; if the property's value is an object, toString() is called on it and the returned value is used.
undefined / null are converted to NULL
NaN / Infinity are left as-is. MySQL does not support these, and trying to insert them as values will trigger MySQL errors until they implement support.
```
##1.

escape()
```
var userId = 'some user provided value';
var sql    = 'SELECT * FROM users WHERE id = ' + connection.escape(userId);
connection.query(sql, function(err, results) {
  // ... 
});
```
跟`?`作用相同
```
connection.query('SELECT * FROM users WHERE id = ?', [userId], function(err, results) {
  // ... 
});
```
##如果要excape的是name

使用escapeId()
```

var a = 'articles';
var sql    = 'SELECT * FROM' + connection.escapeId(a)+ 'WHERE id = 1' ;
connection.query(sql, function(err, results) {
   if (err) {
    console.error(err);
    return;
  }
  console.error(results);
});

```
使用`??`一樣效果

(`??`是給mysql module 內的function解析的，所以他不是變數，不用字串包住前後)
```

var sql    = 'SELECT * FROM ?? WHERE id = 1' ;
connection.query(sql,"articles" ,function(err, results) {
   if (err) {
    console.error(err);
    return;
  }
  console.error(results);
});
```
#insert資料時傳回該筆insert id(主鍵)
```
connection.query('INSERT INTO articles SET ?', {author:'test',title: 'test',body:'test'}, function(err, result) {
  if (err) throw err;
 
  console.log(result.insertId);
});

```

##複習sql語法
```

1. 資料表查詢

 SELECT  `欄位` FROM `資料表`;

一般用法： SELECT * FROM `table` ;

翻譯：選擇table這個資料表所有欄位的資料(就是全選啦!!)

備註：星號代表所有欄位，在sql語法、指令中星號代表全部

 

2. 資料表查詢  +  排序

SELECT  `欄位` FROM `資料表` ORDER BY  `特定欄位` DESC ;

一般用法： SELECT  `id`,`name`  FROM `table` ORDER BY  `特定欄位` DESC ;

翻譯： 選取table資料表內的 id 和 name 這兩個欄位，並根據id這欄位做降冪排序(由高而低、由大到小、由z到a)

備註：ASC則是(由低而高、由小到大、由a到z)，與DESC相反

 

3. 資料表查詢 + 查詢條件

SELECT  `欄位` FROM `資料表` WHERE  `特定欄位` = 數字 ;

一般用法： SELECT  *  FROM `table` WHERE  `id` = 363 ;

翻譯： 在table資料表內的尋找 id 欄位的內容是 363 且將 所有欄位的資料都取出來

 

SELECT  `欄位` FROM `資料表` WHERE  `特定欄位` LIKE  字串 ;

一般用法： SELECT  `id`,`name`  FROM `table` WHERE  `name` LIKE  'admin' ;

 翻譯： 在table資料表內的尋找 name 欄位的內容是 admin 且將 id 和 name 這兩個欄位都取出來

 

SELECT  `欄位` FROM `資料表` WHERE  `特定欄位` LIKE  %字串% ;

一般用法： SELECT  `id`,`name`  FROM `table` WHERE  `name` LIKE  %'adm'% ;

翻譯： 在table資料表內的尋找 name 欄位的內容包含 adm ( admin 符合、administrator 也符合) 且將 id 和 name 這兩個欄位都取出來

 

備註：數字形態比對用 = (也可以用 > 、 < 、 >= 、 <=) ; 字串形態比對是使用 LIKE (LIKE 使用的是完全比對);字串如果需要模糊比對需要使用 %

 

4. 新增(插入)一筆資料

INSERT INTO `資料表`(`欄位1`,`欄位2`) VALUES ( '資料1' , '資料2' );

一般用法： INSERT INTO `table`(`id`,`name`) VALUES ( '12' , 'stanley' );

翻譯： 在 table 資料表內新增一筆資料 在 id 欄位內填入 12 ，在 name 欄位內填入 stanley

備註：在新增一筆資料時，必須將所有欄位和值都填上，預設是空值的欄位值可改成''，且須注意資料表本身的欄位結構、儲存型態，例如： id 欄位禁止存入字串、設有primary屬性的欄位不得輸入空值

 

5. 更新(修改)一筆資料

UPDATE `資料表` SET `欄位2` = '資料2'  WHERE `欄位1` = '資料1'  ;

一般用法：UPDATE `table` SET `name` = 'newaurora'  WHERE `id` = '12'  ;

翻譯： 在 table 資料表內找出 id = 12 的資料，並將 name 欄位內的資料修改為 newaurora

備註：更新資料時必須確定條件設定是否正確，如上例，會把資料表內 id 欄位裡是 12 的資料都找出來並修改成newaurora ，因此使用前必須注意條件判斷
```
#使用完記得關閉
```
connection.end();
```


參考至:

https://www.npmjs.com/package/mysql#getting-the-id-of-an-inserted-row

http://www.sitepoint.com/using-node-mysql-javascript-client/


#於Linux安裝的MySQL

一開始預設只有local可以存取到本地資料夾

停止
```
/etc/init.d/apache2 start
```

開啟
```
sudo /etc/init.d/apache2 start
```

>phpmyadmin中的使用者即為mysql的使用者所以更改後兩者都會變，安裝phpmyadmin套件可參考本書GCE章節。

如在terminal輸入mysql後告知沒有權限，可輸入以下
```
mysql -u root -p
```