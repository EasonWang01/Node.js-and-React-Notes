# socket.io

server
```
var express = require('express');
var app = express();
var server = require('http').createServer(app);
var io =require('socket.io')(server);

// set the view engine to ejs
app.set('view engine', 'ejs');

// public folder to store assets
app.use(express.static(__dirname + '/public'));

// routes for app
app.get('/', function(req, res) {
  res.render('pad');
});
io.on('connection', function(socket){

  console.log('a user connected');



   socket.on('stop', function(socket){
     console.log('sd');
   });





});

// listen on port 8000 (for localhost) or the port defined for heroku
var port = process.env.PORT || 8000;
server.listen(port,function(){console.log("listen 8000")});//記得用socket.io建成的server去監聽
```
client
```
<script src="script.js"></script>
<script src="/socket.io/socket.io.js"></script> <!--記得加入client端 -->
<script>
      var socket = io();//記得產生實例
      socket.emit("stop");
    </script>
```