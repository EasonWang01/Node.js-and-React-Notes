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