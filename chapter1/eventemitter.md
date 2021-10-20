# EventEmitter

1\.

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

2\. Node.js預設最多只能設定10個具有on的回調參數

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

3\. 任何其他的函式都可以使用EventEmitter

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

或者

你也可以用util模組(內建)去做繼承

```
var util = require('util');
var EventEmitter = require('events').EventEmitter;

var Radio = function(station) {

    var self = this;
    console.log(this);
    setTimeout(function() {
       console.log(this);
    }, 0);

    setTimeout(function() {
       console.log(this);
    }, 5000);

    this.on('newListener', function(listener) {
        console.log('Event Listener: ' + listener);
    });

};

util.inherits(Radio, EventEmitter);

var station = {
  freq: '80.16',
  name: 'Rock N Roll Radio',
};

var radio = new Radio(station);

radio.on('open', function(station) {
  console.log('OPEN', station.name, station.freq);

});

radio.on('close', function(station) {
  console.log('CLOSE', station.name, station.freq);
});
```

4\. EventEmitter 原始的事件

newListener事件：添加新的回調函式時觸發。 removeListener事件：移除回調函式時觸發。

```
var EventEmitter = require('events').EventEmitter;
var Event = new EventEmitter;///////記得require後要產生實例

Event.on("newListener", function (evtName){
  console.log("New Listener: " + evtName);
});

Event.on("removeListener", function (evtName){
  console.log("Removed Listener: " + evtName);
});


function foo (){} ///放一個空的function

Event.on("this is on", foo);
Event.removeListener("this is on", foo);
```

5.once方法

```
和on使用方式相同，但他只會監聽一次即移除
```

6.一次移除所有監聽

```
var EventEmitter = require('events').EventEmitter;

var emitter = new EventEmitter;

emitter.removeAllListeners();
```

7.尋找某個事件擁有的回調函式

listener方法

```
var EventEmitter = require('events').EventEmitter;

var ee = new EventEmitter;

function onlyOnce () {

}

ee.on("firstConnection", onlyOnce)
ee.emit("firstConnection");

console.log(ee.listeners("firstConnection"));
```
