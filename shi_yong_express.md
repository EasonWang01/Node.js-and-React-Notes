# 使用express

1.創建一個目錄，再進到該目錄
```
mkdir expressExample

cd expressExample
```


```
npm install express -g --save
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
