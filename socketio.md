#Websocket

####簡介

WebSocket一種在單個 TCP 連線上進行全雙工通訊的協定

以前必須要用Ajax間隔時間去跟server要資料，websocket則可以在建立連線後持續傳送

簡單的比喻：

Ajax 喝水要拿起水杯，喝完要再放下

Websocket 用吸管喝水，要喝時或喝太多要退回去杯子都不必再次拿起水杯

####webSocket相關框架

ws
https://github.com/websockets/ws

engine.io https://github.com/socketio/engine.io


這裡我們使用socket.io當教學範例
# socket.io

```
npm install socket.io --save
```

之後新增server.js

```
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```
index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Websocket Test</title>
    <script src="/socket.io/socket.io.js"></script>
    <script>
    var socket = io();
    </script>
</head>
<body>
    
</body>
</html>
```
之後啟動 `node server.js`

打開瀏覽器`localhost:3000`，並開啟開發人員工具的network觀察

以下取自維基百科https://zh.wikipedia.org/wiki/WebSocket
```
Connection必須設定Upgrade，表示用戶端希望連線升級。
Upgrade欄位必須設定Websocket，表示希望升級到Websocket協定。
Sec-WebSocket-Key是隨機的字串，伺服器端會用這些資料來構造出一個SHA-1的資訊摘要。把「Sec-WebSocket-Key」加上一個特殊字串「258EAFA5-E914-47DA-95CA-C5AB0DC85B11」，然後計算SHA-1摘要，之後進行BASE-64編碼，將結果做為「Sec-WebSocket-Accept」頭的值，返回給用戶端。如此操作，可以儘量避免普通HTTP請求被誤認為Websocket協定。
Sec-WebSocket-Version 表示支援的Websocket版本。RFC6455要求使用的版本是13，之前草案的版本均應當被棄用。
Origin欄位是可選的，通常用來表示在瀏覽器中發起此Websocket連線所在的頁面，類似於Referer。但是，於Referer不同的是，Origin只包含了協定和主機名稱。
其他一些定義在HTTP協定中的欄位，如Cookie等，也可以在Websocket中使用。
```

新增一個事件


server.js

```
io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('chat', function(msg){
    console.log('message: ' + msg);
  });
});
```
index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Websocket Test</title>
    <script src="/socket.io/socket.io.js"></script>
    <script>
      var socket = io();
      function clickBtn() {
        socket.emit('chat',document.getElementById('thisInput').value);
      }
    </script>
</head>
<body>
   <input id="thisInput"> </input>
   <button onclick="clickBtn()">點我</button> 
</body>
</html>
```
可看到我們在client端的方框輸入文字後按送出，可於terminal中看到訊息

接著

剛才是讓server接收client發出的事件，現在試著讓client接收server發送的事件

server.js

```
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('chat', function(msg){
    console.log('message: ' + msg);
    socket.broadcast.emit('message',msg);//傳給所有人除了自己
    socket.emit('message',msg);//傳給自己
  });
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

記得更改完server.js後要記得把server重新啟動

index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Websocket Test</title>
    <script src="/socket.io/socket.io.js"></script>
    <script>
      var socket = io();
      function clickBtn() {
        socket.emit('chat',document.getElementById('thisInput').value);
      }
      //接收事件
      socket.on('message',function(data) {
        document.getElementById('received').innerHTML = data;
      })
    </script>
</head>
<body>
   <input id="thisInput"> </input>
     <button onclick="clickBtn()">點我</button> 
   <div id="received"></div>
</body>
</html>
```


>注意：socket.broadcast.emit會傳給所有connected user除了自己


這時開啟第二個瀏覽器，並在文字框輸入後按送出，即可看到另一個瀏覽器產生文字


#再複習一次，首先必須先再連線範圍作用域才可做事

server.js
```
io.on('connection',(socket) => {
  利用socket來做事
}
```

client就是簡單使用on和emit

#最基本兩種
分別是`socket.on('事件名稱',cb)`和`socket.emit('事件名稱',cb)`

server和client都一樣的用法

`socket.broadcast.emit('user connected');`給所有連線人廣播

#再來是房間部分

`socket.join('房間名稱')`讓client加入房間 

`socket.leave('房間名稱')`讓client離開房間

` socket.broadcast.to('房間名稱').emit('chat',{data: res});`給特定房間廣播訊息

---
#簡單範例
server.js
```
export const socketio = (io, axios, config1) => {

io.on('connection', function(socket){
	console.log('a user connected');

	//房間
	socket.on('mainPage',() => {
		socket.join('mainPage',() => {
		  console.log('join main okok')
			socket.leave('chatPage', () => {
				console.log('leave chat');
			})
		});
	})
	socket.on('chatPage',() => {
		socket.join('chatPage',() => {
		  console.log('join chat')
			socket.leave('mainPage', () => {
				console.log('leave main')
			});
		});
	})


  //事件
  socket.on('chat',(res) => {
    console.log(res);
    socket.broadcast.to('chatPage').emit('chat',{data: res});
    socket.emit('chat',{data: res})
  })

	socket.on('postArticle', function(){
		axios.get(`${config1.origin}/getArticle`)
			.then(function(response){
				socket.broadcast.to('mainPage').emit('addArticle', response.data);//broadcast傳給所有人除了自己
				socket.emit('addArticle', response.data);//加上傳給自己的socket
         //socket.broadcast.to(id).emit('my message', msg);
			}).
			catch(err => {
				console.log(err);
			})
	});
	socket.on('chat', (data) => {
		console.log(data)
	})
});
}

```

###安全機制之token

client端

```
var socket = io.connect('http://localhost:8183/?token=localStorage.getItem('access_token'))
```

server端

```
io.on('connection', function(socket) {
    console.log("url"+socket.handshake.url);
    console.log(socket.handshake.query.token);

});
```
另外client端的cookie會在websocket連線時自動送到server