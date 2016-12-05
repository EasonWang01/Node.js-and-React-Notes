# socket.io


>注意：socket.broadcast.emit會傳給所有connected user除了自己


#首先必須先再連線範圍作用域才可做事

server.js
```
io.on('connection',(socket) => {
  利用socket來做事
}
```

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