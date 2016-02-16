# Stream（流）

一個讀檔案的簡單server為例

```
var http = require('http');
var fs = require('fs');

var server = http.createServer(function (req, res) {
    fs.readFile(__dirname + '/data.txt', function (err, data) {
        res.end(data);
    });
});
server.listen(8000);
```

上面這是第一種寫法，但他會將檔案先緩存到server上的記憶體再一次傳給客戶端，如果檔案太大會造成速度問題

但

如果改成以下這樣

```
var http = require('http');
var fs = require('fs');

var server = http.createServer(function (req, res) {
    var stream = fs.createReadStream(__dirname + '/data.txt');
    stream.pipe(res);
});
server.listen(8000);
```
每次讀到一小段數據後隨即發送給客戶

在cmd輸入node再輸入stream，觀看stream的方法


第一個

Readable

```

var Readable = require('stream').Readable;

var rs = new Readable;
rs.push('beep ');
rs.push('boop\n');
rs.push(null);

rs.pipe(process.stdout);
```
rs.push(null); 告訴Readable要結束了
沒有在結尾輸入的話會產生錯誤

或使用

```
var Readable = require('stream').Readable;

var rs = new Readable;

rs.push('Hi ');

rs.push('Hello\n');

rs.push('World\n');
rs.push(null);


rs.on('data', function(chunk) {
 console.log(chunk);
});

```

但只讀到buffer，如何轉為字串?
```
var Readable = require('stream').Readable;

var rs = new Readable;
rs.setEncoding('utf8');
rs.push('Hi ');

rs.push('Hello\n');

rs.push('World\n');
rs.push(null);

rs.on('data', function(chunk) {
 console.log(chunk);
});
 

```

上面是開啟監聽data，如果想在結束後才一次讀出呢?

監聽end

```
var Readable = require('stream').Readable;
var aa;
var rs = new Readable;
rs.setEncoding('utf8');
rs.push('Hi ');

rs.push('Hello\n');

rs.push('World\n');
rs.push(null);

rs.on('data', function(chunk) {
aa+=chunk;
});
 rs.on('end',function(){
 	console.log(aa);
 })

```