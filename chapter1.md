1.下載Node.js

class.js
```
function hello() {
    console.log('Hello World!');
}
hello();
```
輸入node class

2.模組
export

新增一個class1.js
```
exports.hello1=function () {
    console.log('Hello this is class1!');
}

```
class.js
```
var x = require("./class1.js");

function hello() {
    console.log('Hello World!');
}
hello();
x.hello1();
```
3.module.exports

class1.js
```
module.exports = function () {
    console.log('Hello Hello Hello World!');
};
```

class.js
```
var x = require("./class1.js");

function hello() {
    console.log('Hello World!');
}
hello();

x();
```

4.一個模組中的JS代碼僅在模組第一次被使用時執行一次，並在執行過程中初始化模組的導出對像。之後，緩存起來的導出對像被重複利用。

class1.js
```
var i = 0;

function count() {
    return ++i;
}

exports.count = count;
```
class.js
```
var counter1 = require('./class1.js');
var counter2 = require('./class1.js');

console.log(counter1.count());
console.log(counter2.count());
console.log(counter2.count());
```
5.預設載入路徑(如給予相對路徑沒/)

class.js
```
var counter1 = require('class1.js');
```
將產生無法找到路徑之錯誤

於是新增node_modules資料夾，將class.js放入即可


NodeJS定義了一個特殊的node_modules目錄用於存放模塊。例如某個模塊的絕對路徑是/home/user/hello.js，在該模塊中使用require('foo/bar')方式加載模塊時，則NodeJS依次嘗試使用以下路徑。
```
 /home/user/node_modules/foo/bar
 /home/node_modules/foo/bar
 /node_modules/foo/bar
```

6.手動設定預設加載模組路徑

1.
將以下放在require前
```
module.paths.push("./as", "one/more/path");
```
2.直接更改系統環境變數


7.一次加載(require)在資料夾下的js檔案
 1.第一種方式，將檔案改名為index.js
 創建一個as資料夾下面創一個index.js
 ```
 module.exports = function () {
    console.log('Hello Hello Hello World!');
};
 
 ```
 在外面創一個class.js
 ```
 
var counter1 = require('./as');
 counter1();
 ```
 名稱一定要為index.js才可將路徑指定為其上之資料夾，而非檔案
 
 8.使用package.json
 在as資料夾裡面新增package.json檔案
 ```
 {
    "name": "As you want",
    "main": "class1.js"
}
 ```
 之後即可將index.js改名為class1，而之後就算require的路徑為資料夾，只要其下有package.json檔案，且有指定main(預設開啟檔案)，即可
 
 9.指定require資料夾路徑但其下需載入多個js檔
 ```
 使用require將每個js擋在入口js內載入即可
 ```
 
 10.Build a command tool
 
 http://blog.npmjs.org/post/118810260230/building-a-simple-command-line-tool-with-npm
 
 http://www.ruanyifeng.com/blog/2015/05/command-line-with-node.html