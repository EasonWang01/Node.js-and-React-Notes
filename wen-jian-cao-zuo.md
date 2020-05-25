# 文件操作

## 文件操作

```text
require("fs");
```

1.複製文件

創建一個class2.js檔案

在class.js輸入:

```text
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

```text
  fs = require("fs");

function copy(src, target) {
    fs.createReadStream(src).pipe(fs.createWriteStream(target));
};
copy("./class1.js","./class2.js");
```

2.同步讀取文件

```text
 fs = require("fs");

fileName ="./class1.js";
var text = fs.readFileSync(fileName, "utf8");


console.log(text);
```

3.同步寫入文件

```text
 fs = require("fs");

fileName ="./class2.js";
str="abc";
var text = fs.writeFileSync(fileName,str,"utf8");


console.log(text);
```

發現class2.js整個文件變成abc，如果我只想讓abc附加在文件後呢?

改為appendFileSync

```text
 fs = require("fs");

fileName ="./class2.js";
str="abc";
var text = fs.appendFileSync(fileName,str,"utf8");


console.log(text);
```

4.判斷路徑是否存在

exists\(path, callback\)

```text
 fs = require("fs");

fileName ="./class2.js";
fs.exists(fileName, function (exists) {
 console.log(exists ? "it's there" : "no file!");
});
```

5.移除檔案

先新建一個class3.js

```text
var fs = require('fs');
var filePath = "./class3.js" ; 
fs.unlinkSync(filePath);
```

發現class3.js被移除了

6.移除資料夾

先新建一個名為class3的資料夾

```text
var fs = require('fs');
var filePath = "./class3" ; 
fs.rmdir(filePath);
```

7.創建資料夾

```text
var fs = require('fs');

fs.mkdir('./IamDir',0777, function (err) {
  if (err) throw err;
});
```

但不知道第二個參數是什麼意思，他是指檔案權限的意思，但檔案權限是什麼意思?

下面講一下檔案權限

```text
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

```text
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

多嘗試幾次，發現console的讀取順序不固定  
9.同步讀取

```text
mkdirSync()，writeFileSync()，readFileSync()
```

10.讀取目錄內檔案

```text
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

fs.stat\(path, callback\)

```text
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

watchfile\(\)，unwatchfile\(\)

```text
var fs = require('fs');

fs.watchFile('./class1.js', function (curr, prev) {
  console.log('the current mtime is: ' + curr.mtime);
  console.log('the previous mtime was: ' + prev.mtime);
});
```

執行node class後在class1.js寫上東西後按儲存即會在console顯示訊息

13.createReadStream

createReadStream方法往往用於打開大型的文本文件，創建一個讀取操作的數據流。所謂大型文本文件，指的是文本文件的體積很大，讀取操作的緩存裝不下，只能分成幾次發送，每次發送會觸發一個data事件，發送結束會觸發end事件。

```text
var fs = require('fs');

var input = fs.createReadStream('./class1.js');

input.on('data', function(data) {
console.log(data);

    });
```

會發現得到的是一個buffer

```text
console.log(data.toString());
```

加上後轉為string

另外

```text
data.toString(); 
//還可以指定字元集，預設utf-8
data.toString('ascii');
//更可以Base64 Encode
data.toString('base64'); 
//可以只把部分的資料變成String
data.toString('utf-8',0,10);
```

為什麼要有buffer?

```text
電腦裡有很多檔案其實不是文字檔案來的。實際上，大部分我們開啟的檔案都是二進制檔案，例如圖片檔案，音訊檔案，簡報等，所有檔案裡的資料其實都是以二進制表示的。所以我們就用Buffer物件去表示檔案的內容，和方便我們閱讀檔案資料的每個位元組。
```

```text
1    fs.rename(oldPath, newPath, callback)
異步 rename().回調函數沒有參數，但可能拋出異常。
2    fs.ftruncate(fd, len, callback)
異步 ftruncate().回調函數沒有參數，但可能拋出異常。
3    fs.ftruncateSync(fd, len)
同步 ftruncate()
4    fs.truncate(path, len, callback)
異步 truncate().回調函數沒有參數，但可能拋出異常。
5    fs.truncateSync(path, len)
同步 truncate()
6    fs.chown(path, uid, gid, callback)
異步 chown().回調函數沒有參數，但可能拋出異常。
7    fs.chownSync(path, uid, gid)
同步 chown()
8    fs.fchown(fd, uid, gid, callback)
異步 fchown().回調函數沒有參數，但可能拋出異常。
9    fs.fchownSync(fd, uid, gid)
同步 fchown()
10    fs.lchown(path, uid, gid, callback)
異步 lchown().回調函數沒有參數，但可能拋出異常。
11    fs.lchownSync(path, uid, gid)
同步 lchown()
12    fs.chmod(path, mode, callback)
異步 chmod().回調函數沒有參數，但可能拋出異常。
13    fs.chmodSync(path, mode)
同步 chmod().
14    fs.fchmod(fd, mode, callback)
異步 fchmod().回調函數沒有參數，但可能拋出異常。
15    fs.fchmodSync(fd, mode)
同步 fchmod().
16    fs.lchmod(path, mode, callback)
異步 lchmod().回調函數沒有參數，但可能拋出異常。Only available on Mac OS X.
17    fs.lchmodSync(path, mode)
同步 lchmod().
18    fs.stat(path, callback)
異步 stat(). 回調函數有兩個參數 err, stats，stats 是 fs.Stats 對像。
19    fs.lstat(path, callback)
異步 lstat(). 回調函數有兩個參數 err, stats，stats 是 fs.Stats 對像。
20    fs.fstat(fd, callback)
異步 fstat(). 回調函數有兩個參數 err, stats，stats 是 fs.Stats 對像。
21    fs.statSync(path)
同步 stat(). 返回 fs.Stats 的實例。
22    fs.lstatSync(path)
同步 lstat(). 返回 fs.Stats 的實例。
23    fs.fstatSync(fd)
同步 fstat(). 返回 fs.Stats 的實例。
24    fs.link(srcpath, dstpath, callback)
異步 link().回調函數沒有參數，但可能拋出異常。
25    fs.linkSync(srcpath, dstpath)
同步 link().
26    fs.symlink(srcpath, dstpath[, type], callback)
異步 symlink().回調函數沒有參數，但可能拋出異常。 type 參數可以設置為 'dir', 'file', 或 'junction' (默認為 'file') 。
27    fs.symlinkSync(srcpath, dstpath[, type])
同步 symlink().
28    fs.readlink(path, callback)
異步 readlink(). 回調函數有兩個參數 err, linkString。
29    fs.realpath(path[, cache], callback)
異步 realpath(). 回調函數有兩個參數 err, resolvedPath。
30    fs.realpathSync(path[, cache])
同步 realpath()。返回絕對路徑。
31    fs.unlink(path, callback)
異步 unlink().回調函數沒有參數，但可能拋出異常。
32    fs.unlinkSync(path)
同步 unlink().
33    fs.rmdir(path, callback)
異步 rmdir().回調函數沒有參數，但可能拋出異常。
34    fs.rmdirSync(path)
同步 rmdir().
35    fs.mkdir(path[, mode], callback)
S異步 mkdir(2).回調函數沒有參數，但可能拋出異常。 mode defaults to 0777.
36    fs.mkdirSync(path[, mode])
同步 mkdir().
37    fs.readdir(path, callback)
異步 readdir(3). 讀取目錄的內容。
38    fs.readdirSync(path)
同步 readdir().返回文件數組列表。
39    fs.close(fd, callback)
異步 close().回調函數沒有參數，但可能拋出異常。
40    fs.closeSync(fd)
同步 close().
41    fs.open(path, flags[, mode], callback)
異步打開文件。
42    fs.openSync(path, flags[, mode])
同步 version of fs.open().
43    fs.utimes(path, atime, mtime, callback)

44    fs.utimesSync(path, atime, mtime)
修改文件時間戳，文件通過指定的文件路徑。
45    fs.futimes(fd, atime, mtime, callback)

46    fs.futimesSync(fd, atime, mtime)
修改文件時間戳，通過文件描述符指定。
47    fs.fsync(fd, callback)
異步 fsync.回調函數沒有參數，但可能拋出異常。
48    fs.fsyncSync(fd)
同步 fsync.
49    fs.write(fd, buffer, offset, length[, position], callback)
將緩衝區內容寫入到通過文件描述符指定的文件。
50    fs.write(fd, data[, position[, encoding]], callback)
通過文件描述符 fd 寫入文件內容。
51    fs.writeSync(fd, buffer, offset, length[, position])
同步版的 fs.write()。
52    fs.writeSync(fd, data[, position[, encoding]])
同步版的 fs.write().
53    fs.read(fd, buffer, offset, length, position, callback)
通過文件描述符 fd 讀取文件內容。
54    fs.readSync(fd, buffer, offset, length, position)
同步版的 fs.read.
55    fs.readFile(filename[, options], callback)
異步讀取文件內容。
56    fs.readFileSync(filename[, options])
57    fs.writeFile(filename, data[, options], callback)
異步寫入文件內容。
58    fs.writeFileSync(filename, data[, options])
同步版的 fs.writeFile。
59    fs.appendFile(filename, data[, options], callback)
異步追加文件內容。
60    fs.appendFileSync(filename, data[, options])
The 同步 version of fs.appendFile.
61    fs.watchFile(filename[, options], listener)
查看文件的修改。
62    fs.unwatchFile(filename[, listener])
停止查看 filename 的修改。
63    fs.watch(filename[, options][, listener])
查看 filename 的修改，filename 可以是文件或目錄。返回 fs.FSWatcher 對像。
64    fs.exists(path, callback)
檢測給定的路徑是否存在。
65    fs.existsSync(path)
同步版的 fs.exists.
66    fs.access(path[, mode], callback)
測試指定路徑用戶權限。
67    fs.accessSync(path[, mode])
同步版的 fs.access。
68    fs.createReadStream(path[, options])
返回ReadStream 對像。
69    fs.createWriteStream(path[, options])
返回 WriteStream 對像。
70    fs.symlink(srcpath, dstpath[, type], callback)
異步 symlink().回調函數沒有參數，但可能拋出異常。
```

## fs.utimes

```text
修改檔案的atime與mtime，
```

其他可參考下面文章

[https://www.shellhacks.com/fake-file-access-modify-change-timestamps-linux/](https://www.shellhacks.com/fake-file-access-modify-change-timestamps-linux/)

