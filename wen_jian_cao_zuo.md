# 文件操作
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
多嘗試幾次，發現console的讀取順序不固定
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

11.