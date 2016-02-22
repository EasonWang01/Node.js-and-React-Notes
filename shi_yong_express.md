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

```
forever --watch index.js
```



一個簡單的範例

```
var express = require('express');
var path = require('path');

var app = express();

app.use(express.static('./dist'));

app.use('/', function (req, res) {
    res.sendFile(path.resolve('client/index.html'));
});

var port = 3000;

app.listen(port, function(error) {
  if (error) throw error;
  console.log("Express server listening on port", port);
});
```
