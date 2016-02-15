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