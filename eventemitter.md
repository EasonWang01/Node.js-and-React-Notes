# EventEmitter
1.

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
var oneEvent = new EventEmitter();

oneEvent.on('an Event', function () {
  console.log('event has occured');
});

function f() {
  console.log('start');
  oneEvent.emit('an Event');
  console.log('end');
}

f()
// start
// event has occured
// end
```
一個emit發出一個用on接收

Socket.io就是使用這個概念

2.
Node.js預設最多只能設定10個具有on的回調參數
```
var EventEmitter = require('events').EventEmitter;
var oneEvent = new EventEmitter();

oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
oneEvent.on('an Event', function () {
  console.log('event has occured');
});
function f() {
  console.log('start');
  oneEvent.emit('an Event');
  console.log('end');
}

f()
// start
// event has occured
// end
```
代碼改成上面後會發現console出現提醒訊息


但

我們可以加入一行來手動增加監聽器on的數量

```
oneEvent.setMaxListeners(20);
```

2.可以在emit傳入參數
```
var EventEmitter = require('events').EventEmitter;
var myEmitter = new EventEmitter;

var connection = function(id,ad){
  console.log('client id: ' + id+ad);
};

myEmitter.on('connection', connection);
myEmitter.emit('connection', 6,8);
```

3.
任何其他的函式都可以使用EventEmitter

只要將它繼承即可

```
var EventEmitter = require('events').EventEmitter;

function Dog(name) {
  this.name = name;
}

Dog.prototype.__proto__ = EventEmitter.prototype;
// 另一種寫法
// Dog.prototype = Object.create(EventEmitter.prototype);

var simon = new Dog('simon');

simon.on('bark', function(){
  console.log(this.name + ' barked');
});

setInterval(function(){
  simon.emit('bark');
}, 500);
```