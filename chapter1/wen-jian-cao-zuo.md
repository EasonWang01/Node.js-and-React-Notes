# fs 文件操作

## 文件操作

```
require("fs");
```

1.複製文件

創建一個class2.js檔案

在class.js輸入:

```
fs = require("fs");

function copy(src, target) {
    console.log(target);
    fs.writeFileSync(target, fs.readFileSync(src));
}
copy("./class1.js","./class2.js");
```

但這樣的寫法容易產生內存溢出，以及同步阻塞

可改成

class.js

```
  fs = require("fs");

function copy(src, target) {
    fs.createReadStream(src).pipe(fs.createWriteStream(target));
};
copy("./class1.js","./class2.js");
```

2.同步讀取文件

```
 fs = require("fs");

fileName ="./class1.js";
var text = fs.readFileSync(fileName, "utf8");


console.log(text);
```

3.同步寫入文件

```
 fs = require("fs");

fileName ="./class2.js";
str="abc";
var text = fs.writeFileSync(fileName,str,"utf8");


console.log(text);
```

發現class2.js整個文件變成abc，如果我只想讓abc附加在文件後呢?

改為appendFileSync

```
 fs = require("fs");

fileName ="./class2.js";
str="abc";
var text = fs.appendFileSync(fileName,str,"utf8");


console.log(text);
```

4.判斷路徑是否存在

exists(path, callback)

```
 fs = require("fs");

fileName ="./class2.js";
fs.exists(fileName, function (exists) {
 console.log(exists ? "it's there" : "no file!");
});
```

5.移除檔案

先新建一個class3.js

```
var fs = require('fs');
var filePath = "./class3.js" ; 
fs.unlinkSync(filePath);
```

發現class3.js被移除了

6.移除資料夾

先新建一個名為class3的資料夾

```
var fs = require('fs');
var filePath = "./class3" ; 
fs.rmdir(filePath);
```

7.創建資料夾

```
var fs = require('fs');

fs.mkdir('./IamDir',0777, function (err) {
  if (err) throw err;
});
```

但不知道第二個參數是什麼意思，他是指檔案權限的意思，但檔案權限是什麼意思?

下面講一下檔案權限

```
讀取 Read = R = 4
寫入 Write = W = 2
執行 eXecute = X = 1

Owner = 程式擁有人
Group = 同一群組的其它帳號
Other = 全世界，如訪客


(1) 如果您要開放檔案給任何人 "讀取"、並能 "寫入"，那麼：

Owner (程式擁有人) = 4 + 2 + 0 = 6
Group (同一群組) = 4 + 2 + 0 = 6
Other (訪客) = 4 + 2 + 0 = 6

所以 Owner Group Other = 666

(2) 如果您要開放檔案給任何人執行，自己可以更改檔案，但是不希望其它人更改您的檔案，那麼：

Owner (程式擁有人) = 4 + 2 + 1 = 7
Group (同一群組) = 4 + 0 + 1 = 5
Other (訪客) = 4 + 0 + 1 = 5

所以 Owner Group Other = 755
```

8.readFile是異步的

```
var fs = require('fs');

fs.readFile('./class1.js','UTF-8' ,function (err, data) {
  if (err) throw err;
  console.log(data);
});
fs.readFile('./class2.js','UTF-8' ,function (err, data) {
  if (err) throw err;
  console.log(data);
});
fs.readFile('./class3.js','UTF-8' ,function (err, data) {
  if (err) throw err;
  console.log(data);
});
```

多嘗試幾次，發現console的讀取順序不固定\
9.同步讀取

```
mkdirSync()，writeFileSync()，readFileSync()
```

10.讀取目錄內檔案

```
var fs = require('fs');

dir="./as";
fs.readdir(dir, function (err, files) {
  if (err) {
    console.log(err);
    return;
  };

  console.log(files);


});
```

會返回一個檔案名稱產生的陣列

11.查看檔案詳細資訊

fs.stat(path, callback)

```
var fs = require('fs');

dir="./as";
fs.readdir(dir, function (err, files) {
  if (err) {

    return;
  };

  console.log(files);

  files.forEach( function (file) {
    fs.stat('./as/' + file, function (err, stats) {
      if (err) throw err;
    console.log(stats);
   });
});

});
```

12.監聽文件

watchfile()，unwatchfile()

```
var fs = require('fs');

fs.watchFile('./class1.js', function (curr, prev) {
  console.log('the current mtime is: ' + curr.mtime);
  console.log('the previous mtime was: ' + prev.mtime);
});
```

執行node class後在class1.js寫上東西後按儲存即會在console顯示訊息

13.createReadStream

createReadStream方法往往用於打開大型的文本文件，創建一個讀取操作的數據流。所謂大型文本文件，指的是文本文件的體積很大，讀取操作的緩存裝不下，只能分成幾次發送，每次發送會觸發一個data事件，發送結束會觸發end事件。

```
var fs = require('fs');

var input = fs.createReadStream('./class1.js');

input.on('data', function(data) {
console.log(data);

    });
```

會發現得到的是一個buffer

```
console.log(data.toString());
```

加上後轉為string

另外

```
data.toString(); 

//還可以指定字元集，預設utf-8
data.toString('ascii');

//更可以Base64 Encode
data.toString('base64'); 

//可以只把部分的資料變成String
data.toString('utf-8',0,10);
```

為什麼要有buffer?

```
電腦裡有很多檔案其實不是文字檔案來的。實際上，大部分我們開啟的檔案都是二進制檔案，例如圖片檔案，音訊檔案，簡報等，所有檔案裡的資料其實都是以二進制表示的。所以我們就用Buffer物件去表示檔案的內容，和方便我們閱讀檔案資料的每個位元組。
```

## fs.utimes

```
修改檔案的atime與mtime，
```

其他可參考下面文章

{% embed url="https://www.shellhacks.com/fake-file-access-modify-change-timestamps-linux/" %}

## 讀取出所有資料夾內的資料夾

```javascript
const fs = require("fs");

let result = [];

const isDirectory = path => (
  fs.lstatSync(path).isDirectory()
)

const isBlockList = dirName => {
  const blackList = ['__snapshots__', 'test']
  return blackList.some(name => name === dirName);
}

const readDeepDir = (path) => {
  if(!isDirectory(path)) {
    return
  }

  const dir = fs.readdirSync(path);
  if (dir.length > 1) {
    dir.forEach(_dir => {
      const absolutePath = path + '/' + _dir;
      if(isDirectory(absolutePath) && !isBlockList(_dir)) {
        result.push(absolutePath);
      }
      readDeepDir(absolutePath);
    });
  }
  return result;
};

const dir = readDeepDir('../../aip/src/web/console/src/components');
console.log(dir)

```

## 不使用第三方模組解析上傳檔案

> 第三方模組 e.g. formidable, body-parser

index.html

```markup
<!DOCTYPE html>
<html>
<input id="fileInput" type="file"></input>
<button onclick="uploadFile()">上傳</button>
<script>
    function uploadFile() {
        const fileInput = document.querySelector('#fileInput');
        const file = fileInput.files[0];
        const reader = new FileReader();
        reader.readAsArrayBuffer(file);
        reader.onload = function () {
            const arrayBuffer = reader.result
            const bytes = new Uint8Array(arrayBuffer);
            console.log(bytes)
            fetch('http://localhost:5000', {
                method: 'POST', // or 'PUT'
                body: JSON.stringify({
                  file: bytes
                }), // data can be `string` or {object}!
                headers: new Headers({
                    'Content-Type': 'application/json'
                })
            }).then(res => res.json())
                .catch(error => console.error('Error:', error))
                .then(response => console.log('Success:', response));
        }
    }
</script>

</html>
```

server.js

> 這邊從 utf8 buffer array 轉為string 可參考 [https://stackoverflow.com/questions/8936984/uint8array-to-string-in-javascript](https://stackoverflow.com/questions/8936984/uint8array-to-string-in-javascript)

```javascript
const http = require('http');

const setCORS = () => {
  const headers = {};
  headers["Access-Control-Allow-Origin"] = "*";
  headers["Access-Control-Allow-Methods"] = "POST, GET, PUT, DELETE, OPTIONS";
  headers["Access-Control-Allow-Credentials"] = false;
  headers["Access-Control-Max-Age"] = '86400'; // 24 hours
  headers["Access-Control-Allow-Headers"] = "X-Requested-With, X-HTTP-Method-Override, Content-Type, Accept";
  return headers
}

const parseBody = (req, callback) => {
  let body = '';
  req.on('data', chunk => {
    body += chunk.toString();
  });
  req.on('end', () => {
    callback(body);
  });
}

http.createServer(function (req, res) {
  if (req.method === 'OPTIONS') {
    const headers = setCORS();
    res.writeHead(200, headers);
    res.end();
  }
  if (req.url === '/') {
    if (req.method === "POST") {
      parseBody(req, (body) => {
        console.log(Utf8ArrayToStr(Object.values(JSON.parse(body).file)));
        // Utf8ArrayToStr 換成用 Buffer.from 也可以
        const headers = setCORS();
        res.writeHead(200, headers);
        res.end(JSON.stringify({ result: 'ok' }));
      })
    }
  }
}).listen(5000);


function Utf8ArrayToStr(array) {
  var out, i, len, c;
  var char2, char3;

  out = "";
  len = array.length;
  i = 0;
  while (i < len) {
    c = array[i++];
    switch (c >> 4) {
      case 0: case 1: case 2: case 3: case 4: case 5: case 6: case 7:
        // 0xxxxxxx
        out += String.fromCharCode(c);
        break;
      case 12: case 13:
        // 110x xxxx   10xx xxxx
        char2 = array[i++];
        out += String.fromCharCode(((c & 0x1F) << 6) | (char2 & 0x3F));
        break;
      case 14:
        // 1110 xxxx  10xx xxxx  10xx xxxx
        char2 = array[i++];
        char3 = array[i++];
        out += String.fromCharCode(((c & 0x0F) << 12) |
          ((char2 & 0x3F) << 6) |
          ((char3 & 0x3F) << 0));
        break;
    }
  }

  return out;
}

console.log('Server running on port 5000.');
```

> 但之後會發現檔案太大還是會 browser crash，大約 4mb以上，所以還是要用form/data來傳，並用 formidable 解析，如果不用其他模組仍可解析，但要自己parse  ------WebKitFormBoundary 內的內容
>
> 因為 form/data 可以直接傳入 file input 的 files\[0] 類行為 blob，但如果要用 post 必須在 body 傳入 arraybuffer，如果太大會導致 browser crash

## 上傳 file 轉為 stream

假設檔案長這樣

![](<../.gitbook/assets/螢幕快照 2020-06-19 上午9.29.05.png>)

此時可用&#x20;

```javascript
fs.createReadStream(file.path)
```

將其轉為 stream 然後上傳到 minio 或 S3 之類地方。

## 將檔案從server 傳到 client

1.可用 express 之 res.download('檔案路徑')

或是使用

```javascript
async function get (req, res) {
  const fileName = 'model-sample-1.1_new.json';
  const readStream = new stream.PassThrough();
  const file = fs.readFileSync(`${__dirname}/${fileName}`);
  readStream.end(Buffer.from(file));

  res.set('Content-disposition', 'attachment; filename=' + fileName);

  readStream.pipe(res);
}
```

在前端部分可以如下下載檔案

```javascript
window.open(toFullUrl('api/download/template'));
```

## 前端取得上傳檔案進度

```javascript

const request = new XMLHttpRequest();
request.open("POST", "http://localhost:3923/file");
request.upload.addEventListener(
  "progress",
  function(evt) {
    if (evt.lengthComputable) {
    var percentComplete = evt.loaded / evt.total;
    console.log(percentComplete);
  }
},false);
request.send(formData);
```

axios 取得上傳進度

```javascript
axios.post(toFullUrl('api/upload'), formData, {
  onUploadProgress: (progressEvent) => {
  const percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total)
  console.log(percentCompleted)
}
}).then(res => {
...
})
```

> Fetch API 目前還無法取得上傳進度

## 大檔案切片上傳

在前端把檔案切分，分成多的請求上傳，然後在後端合併。

{% embed url="https://github.com/23/resumable.js" %}

clone 之後進入範例：`resumable.js/samples/Node.js`

然後把這幾個部分改一下：

前端

```
var r = new Resumable({
  target:'http://localhost:3000/upload',
  chunkSize:1*1024*1024,
  simultaneousUploads:4,
  testChunks:false,
  throttleProgressCallbacks:1
});
```

後端

_app.js_ has been tweaked to send `resumable` a directory where to save the file (top most).

```
var resumable = require('./resumable-node.js')(__dirname + "/uploads");
```

tweaked _app.js_ to change the content of `app.post('/uploads',...)` see [gist](https://gist.github.com/rhamedy/9607ce395f3cb1b758eb).

```
// Handle uploads through Resumable.js
app.post('/upload', function(req, res){
  resumable.post(req, function(status, filename, original_filename, identifier){
    if (status === 'done') {
      var stream = fs.createWriteStream('./uploads/' + filename);

      //stich the chunks
      resumable.write(identifier, stream);
      stream.on('data', function(data){});
      stream.on('end', function(){});

      //delete chunks
      resumable.clean(identifier);
    }
    res.send(status, {
        // NOTE: Uncomment this funciton to enable cross-domain request.
        //'Access-Control-Allow-Origin': '*'
    });
  });
});
```

[https://stackoverflow.com/a/35137586/4622645](https://stackoverflow.com/a/35137586/4622645)

## 產生大檔案

```javascript
const fs = require('fs');

const GB = 1;

const byteSize = 1000 * 1000 * 1000 * GB;

fs.writeFileSync('./bigfile.txt', Buffer.alloc(byteSize));
```

> 1024 or 1000&#x20;
>
> [https://stackoverflow.com/questions/8632269/displaying-file-size-1000b-1kb-or-1024b-1kb](https://stackoverflow.com/questions/8632269/displaying-file-size-1000b-1kb-or-1024b-1kb)
