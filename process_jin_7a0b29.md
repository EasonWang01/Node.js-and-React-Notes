# Process (進程)

process.stdout和process.stdin都是stream的實例

```
 process.stdout.write("ass");

```
等於
```
console.log("ass")
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
返回一個陣列，參數從process.argv[2]開始


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
