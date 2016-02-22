# 使用express

1.創建一個目錄，再進到該目錄
```
mkdir expressExample

cd expressExample
```
2.初始化
```
npm init
```
3.
```
npm install express --save
```
先創建一個index.js
```
var express = require('express');
var app = express();

app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/

app.listen(8080);
```
##但下面這是什麼?
```
app.use(express.static(__dirname + '/public'));
```
##試著把你剛才創的index.js複製一個到public資料夾，之後在網址打上http://localhost:8080/index.js


可以設定多個靜態目錄
```
app.use(express.static('public'));
app.use(express.static('img'));
app.use(express.static('pdf'));
```

4.我們在index.js 內加入下面，再執行看看
```
app.get('/', function (req, res) {
  res.send('Hello world!');
});
app.get('/hi', function (req, res) {
  res.send('Hello world!');
});
```
##每次修改完都要重新啟動server覺得很麻煩

所以我們要安裝一個套件:forever

```
npm install forever --save
```
開始監控
```
forever --watch index.js
```

----------------------
5.接著我們加入更多路由功能
```
app.get('/', function (req, res) {
  res.send('Hello wogd!');
});
app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });
```
但發現這樣會很雜亂，所以我們新建一個目錄叫routes

裡面放入一個檔案index.js
```
module.exports = function (app) {
app.get('/', function (req, res) {
  res.send('Hello wogd!');
});
app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Hello world');
  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};
```
而 原本的檔案改為
```
var express = require('express');
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/

app.listen(8080);
```
這時我們試著把
```
var router = require('./routes/index.js')(app);
改為
var router = require('./routes')(app);

```
發現還是可以，原因是require如果指定為資料夾，他會預設去找下面的index檔案

express 是一個架構在http上的框架

#middleware

接收http請求，並對其進行加工，記得調用next，表示繼續往下加工
```
function IamMiddleware(req, res, next) {
  next();
}
```
有哪些種類?
```
應用程式層次的中介軟體
路由器層次的中介軟體
錯誤處理中介軟體
內建中介軟體
協力廠商中介軟體
```
應用程式層次的中介軟體，將下面code貼在我們的index.js看看
```
app.use(function (req, res, next) {
  console.log('現在時間:', Date.now());
  next();
});
```
限定指定路徑使用
```
app.get('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});
```
```
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method);
  console.log(req.params.id);
  next();
});
```
我們可以指定條件再middleware，先在原本middleware後加入一個function，

指定如果url的params = 條件的話跳過其下逗點後的function直接進行next middleware
```
var express = require('express');
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/
app.get('/user/:id', function (req, res, next) {
  // 如果id參數為0跳到下面的middleware
  if (req.params.id == 0) next('route');
   
  else next(); 
}, function (req, res, next) {
  
  console.log("I am this")
  next(); 
});

// handler for the /user/:id path, which renders a special page
app.get('/user/:d', function (req, res, next) {
  
  console.log("I am next")
  next();
});
app.listen(8080);
```
發現如果url為
```
http://localhost:8080/user/hi
```
將console出 
```
I am this 

I am next
```
如果url為
```
http://localhost:8080/user/0
```
將console出 
```
I am next
```
#整理路徑router
1.index.js
```

var fruit = require('./fruit');
app.use('/fruit', fruit);
```
2.設定router檔案
```
var express = require('express');
var fruit = express.Router();

fruit.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});

fruit.get('/', function(req, res) {
  res.send('Birds home page');
});

fruit.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = fruit;
```