# Process (進程)

## Process (進程)

process.stdout和process.stdin都是stream的實例

```
 process.stdout.write("test");
```

等於

```
console.log("test")
```

因為

```
console.log = function(d) {
  process.stdout.write(d + '\n');
};
```

讀取檔案輸出到terminal，pipe流動時會自動write

```
 var fs = require('fs');

fs.createReadStream('./class1.js')
  .pipe(process.stdout);
```

基本

```
process.stdin.on('data', function(chunk) {

    process.stdout.write('data: ' + chunk);

});

process.stdin.on('end', function() {
  process.stdout.write('end');
});
```

使用流

```
process.stdin.pipe(process.stdout)
```

設定編碼格式

```
process.stdin.setEncoding('utf8');
```

取得檔案位置

```
console.log("argv: ",process.argv);
```

如果在後面加上參數呢?

```
執行 node class 123
```

返回一個陣列，參數從process.argv\[2]開始

另外

```
console.log(process.execArgv);
```

```
執行 node --harmony class.js
```

將返回--harmony

另外

試試

```
console.log(process.env);
```

以及

```
process.chdir()：切換工作目錄到指定目錄。
process.cwd()：返回運行當前腳本的工作目錄的路徑。
process.exit()：退出當前進程。
process.getgid()：返回當前進程的組ID（數值）。
process.getuid()：返回當前進程的用戶ID（數值）。
process.nextTick()：指定回調函數在當前執行棧的尾部、下一次Event Loop之前執行。
process.on()：監聽事件。
process.setgid()：指定當前進程的組，可以使用數字ID，也可以使用字符串ID。
process.setuid()：指定當前進程的用戶，可以使用數字ID，也可以使用字符串ID。
```

## process.nextTick()

非同步的幫手 其類似

```
setTimeout(function () {
  console.log('沒有其他延遲的話我通常最後執行！');
}, 0)
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
```

```
process.nextTick(function () {
  console.log('沒有其他延遲的話我通常最後執行！');
}, 0)

 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
```

如果nextTick跟setTimeout 放一起nextTick會先執行

```
setTimeout(function () {
  console.log('沒有其他延遲的話我通常最後執行setTimeout！');
}, 0)
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');
 console.log('執行！');

process.nextTick(function () {
  console.log('沒有其他延遲的話我通常最後執行！');
}, 0)
```

## uncaughtException

當前進程拋出一個沒有被catch的錯誤時，會觸發uncaughtException事件。

```
process.on('uncaughtException', function (err) {
  console.error('An uncaught error occurred!');
  console.error(err.stack);
  throw new Error('產生錯誤');
});
```

非常正常，沒有產生任何訊息

改成下面

```
process.on('uncaughtException', function (err) {
  console.error('An uncaught error occurred!');
  console.error(err.stack);
  throw new Error('產生錯誤');
});

throw new Error('something wrong');
```

uncaughtException事件，是免於Node進程終止的最後措施，否則Node就要執行process.exit()。
