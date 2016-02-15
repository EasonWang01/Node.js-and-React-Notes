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
  
  class.js
  
  ```
  fs = require("fs");

function copy(src, target) {
    fs.createReadStream(src).pipe(fs.createWriteStream(target));
};
copy("./class1.js","./class2.js");
  ```