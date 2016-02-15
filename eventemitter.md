# EventEmitter
剛才在操作檔案時，看到類似這樣的東西
```
fs.on('open', function () {
  console.log('event has occured');
});
```
原因是當我們在讀取時他會自動發出
```
fs.emit('open');
```
而
```
fs.on("open")接到他的通知
```

實際上他已經繼承了EventEmitter

以下為一個範例

```
var EventEmitter = require('events').EventEmitter;
var ee = new EventEmitter();

ee.on('someEvent', function () {
  console.log('event has occured');
});

function f() {
  console.log('start');
  ee.emit('someEvent');
  console.log('end');
}

f()
// start
// event has occured
// end
```