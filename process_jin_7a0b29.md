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