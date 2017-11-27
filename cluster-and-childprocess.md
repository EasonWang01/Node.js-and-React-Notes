# Cluster and Child\_process

[https://nodejs.org/api/cluster.html](https://nodejs.org/api/cluster.html)

因為Nodejs為單進程，所以預設只會用一個ＣＰＵ執行程式，所以我們可以用Cluster來讓他可以跑多個CPU

1.以下為讓每個cpu都跑一個process的範例\(根據cpu數量\)

```js
var cluster = require('cluster');
var os = require('os');

if (cluster.isMaster){
  for (var i = 0, n = os.cpus().length; i < n; i += 1){
    cluster.fork();
  }
} else {
  http.createServer(function(req, res) {
    res.writeHead(200);
    res.end("hello world\n");
  }).listen(8000);
}
```

2.以下為讓子進程與主進程溝通的範例

```js
var cluster = require('cluster');
var http = require('http');
var numReqs = 0;
var workers = [];

if (cluster.isMaster) {
  // Broadcast a message to all workers
  var broadcast = function() {
    for (var i in workers) {
      var worker = workers[i];
      worker.send({ cmd: 'broadcast', numReqs: numReqs }); //send裡面可以隨便放
    }
  }

  // Fork workers.
  for (var i = 0; i < 2; i++) {
    var worker = cluster.fork();

    worker.on('message', function(msg) {
      if (msg.cmd) {
        switch (msg.cmd) {
          case 'notifyRequest':
            numReqs++;
          break;
          case 'broadcast':
            broadcast();
          break;
        }
    });

    // Add the worker to an array of known workers
    workers.push(worker);
  }

  setInterval(function() {
    console.log("numReqs =", numReqs);
  }, 1000);
} else {
  // React to messages received from master
  process.on('message', function(msg) {
    switch(msg.cmd) {
      case 'broadcast':
        if (msg.numReqs) console.log('Number of requests: ' + msg.numReqs);
      break;
    }
  });

  // Worker processes have a http server.
  http.Server(function(req, res) {
    res.writeHead(200);
    res.end("hello world\n");
    // Send message to master process
    process.send({ cmd: 'notifyRequest' });
    process.send({ cmd: 'broadcast' });
  }).listen(8000);
}
```



但是

> master 進程創建 socket，綁定到某個地址以及PORT後，自身不調用 listen 來監聽連接以及 accept 連接，而是將該 socket 的 fd\(file descriptor\) 傳遞到 fork 出來的 worker 進程，worker 接收到 fd 後再調用 listen，accept 新的連接。但實際一個新到來的連接最終只能被某一個 worker 進程 accpet 再做處理。
>
> 所以會造成子進程產生競爭現象
>
> 可參考：http://taobaofed.org/blog/2015/11/03/nodejs-cluster/



