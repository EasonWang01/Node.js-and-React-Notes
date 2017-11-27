# Cluster and Child\_process

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





