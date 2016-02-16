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