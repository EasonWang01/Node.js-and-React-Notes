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


接續上一章
pad.ejs
```

<!DOCTYPE html>
<html>
<head>
    <title>Realtime Markdown Viewer</title>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css" rel="stylesheet">
    <link href="style.css" rel="stylesheet">
</head>

<body class="container-fluid">

    <section class="row">
        <textarea class="col-md-6 full-height" id="pad">Write your text here..</textarea>
        <div class="col-md-6 full-height" id="markdown"></div>
    </section>

    <script src="https://cdn.rawgit.com/showdownjs/showdown/1.0.2/dist/showdown.min.js"></script>

<script src="/socket.io/socket.io.js"></script> <!--記得加入client端 -->
   <script src="script.js"></script>

</body>
</html>
```
script.js
```
window.onload = function() {

      var socket = io();//記得產生實例
     

    var converter = new showdown.Converter();
    var pad = document.getElementById('pad');
    var markdownArea = document.getElementById('markdown');   

    var convertTextAreaToMarkdown = function(){
        var markdownText = pad.value;
        html = converter.makeHtml(markdownText);
        markdownArea.innerHTML = html;

        socket.emit("userinput",{data:html})
        };


    pad.addEventListener('input', convertTextAreaToMarkdown);

    convertTextAreaToMarkdown();
};
```
server.js
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



   socket.on('userinput', function(data){
     console.log(data.data);
     
   });





});

// listen on port 8000 (for localhost) or the port defined for heroku
var port = process.env.PORT || 8000;
server.listen(port,function(){console.log("listen 8000")});//記得用socket.io建成的server去監聽
```
之後打開terminal對照網頁輸入，可發現輸入隨即出現於terminal上。


之後要讓兩個client(瀏覽器)都接的到訊息
新增`io.sockets.emit('updateClient',{data:data})`
```
 socket.on('userinput', function(data){
     console.log(data.data);
     io.sockets.emit('updateClient',{data:data})
   });
```
而client端
```
window.onload = function() {

      var socket = io();//記得產生實例
      
  

    var converter = new showdown.Converter();
    var pad = document.getElementById('pad');
    var markdownArea = document.getElementById('markdown');   

    var convertTextAreaToMarkdown = function(){
        var markdownText = pad.value;
        html = converter.makeHtml(markdownText);
        markdownArea.innerHTML = html;

        socket.emit("userinput",{data:html})
        };

        socket.on('updateClient',function(data){

            console.log( data.data.data);
           // pad.innerHTML = data.data.data;
            
        });

    pad.addEventListener('input', convertTextAreaToMarkdown);

    convertTextAreaToMarkdown();
};
```
之後開啟兩個瀏覽器，並打開console查看

##但
發現都有HTML　tag　在外面，我們的目的是讓所有client端同步
　所以我們要擷取tag內的字串
 ```
 
console.log((data.data.data).length);
var res =(data.data.data).slice(3,(data.data.data).length-4);
 ```