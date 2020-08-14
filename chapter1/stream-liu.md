# Stream（流）

Stream（流）

具有readable、writable、drain、data、end、close等事件

一個讀檔案的簡單server為例

```text
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

```text
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

```text
var Readable = require('stream').Readable;

var rs = new Readable;
rs.push('beep ');
rs.push('boop\n');
rs.push(null);

rs.pipe(process.stdout);
```

rs.push\(null\); 告訴Readable要結束了  
沒有在結尾輸入的話會產生錯誤

或使用

```text
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

```text
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

```text
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

如何讓他暫停不要結束?

pause\(\);

```text
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

rs.pause();

 rs.on('end',function(){
     console.log(aa);
 })
```

如何確認是不是暫停?

```text
console.log(rs.isPaused());
```

暫停後如何繼續?

```text
rs.resume();
```

讀進buffer後如何寫出?

```text
fs.writeFile(filename, data, [encoding], [callback])
```

範例

```text
var Readable = require('stream').Readable;
var fs = require('fs');

var rs = new Readable;

rs.push('Hi ');
rs.push(null);

rs.on('data', function(chunk) {
     console.log(chunk);
     fs.writeFile('./class1.js',chunk)
});
```

有更高級的寫法嗎?

使用write的stream方法

```text
var Readable = require('stream').Readable;
var fs = require('fs');

var rs = new Readable;

rs.push('Hi ');
rs.push(null);

rs.on('data', function(chunk) {
     console.log(chunk);
     fs.createWriteStream('./class1.js').write(chunk);
});
```

將

```text
fs.createWriteStream('./class1.js').write(chunk);
```

改成

```text
console.log(fs.createWriteStream('./class1.js').write(chunk))
```

猜看看會出現什麼?

如何控制寫入?

```text
write()
end()
```

## Pipe

慢慢寫太慢了，直接用管子讓他快速流過去

前提: read和write的實例要繼承stream

```text
pipe()
unpipe()
```

## Stream 種類

```text
Writable: streams to which data can be written (for example, fs.createWriteStream()).
Readable: streams from which data can be read (for example, fs.createReadStream()).
Duplex: streams that are both Readable and Writable (for example, net.Socket).
Transform: Duplex streams that can modify or transform the data as it is written and read (for example, zlib.createDeflate()).
```

## Stream backpresure

當水管的入口大於水管的出口時，會讓多個水從水管口溢出，Node.js pipe 是雙工的，並且會處理相關 backpresure

> In any scenario where the data buffer has exceeded the \[`highWaterMark`\]\[\] or the write queue is currently busy, \[`.write()`\]\[\] will return `false`.
>
> When a `false` value is returned, the backpressure system kicks in. It will pause the incoming \[`Readable`\]\[\] stream from sending any data and wait until the consumer is ready again. Once the data buffer is emptied, a \[`'drain'`\]\[\] event will be emitted and resume the incoming data flow.

[https://nodejs.org/es/docs/guides/backpressuring-in-streams/](https://nodejs.org/es/docs/guides/backpressuring-in-streams/)



