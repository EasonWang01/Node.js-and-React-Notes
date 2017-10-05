# http

```
var http = require('http');

http.createServer(function (request, response){
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
}).listen(3000);

console.log('Server running on port 3000.');
```

讀檔案

```
var http = require('http');
var fs = require('fs');

http.createServer(function (request, response){
  fs.readFile('class1.js', function readData(err, data) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end(data);
  });
}).listen(3000);

console.log('Server running on port 3000.');
```

讀HTML

```
將'Content-Type': 'text/plain' 改為'Content-Type': 'text/html'
```



# \#取得remote ip





> 注意 如果是在proxy後面 例如nginx

```js
const http = require('http');
const server = http.createServer((req, res) => {
const util = require('util');

  const port = res.socket.remotePort;
  var ip;

console.log('x-forward for: ' + req.headers['x-forwarded-for'])
console.log('socket remote Address: '+ req.socket.remoteAddress)
console.log(' req.connection.socket.remoteAddress: ' +  req.connection.socket)

// console.log(req.connection.remoteAddress)
// console.log(util.inspect(req.connection, false, null))
// console.log('01' + req.connection);
console.log('req.ip: ' + req.ip)
  if (req.headers['x-forwarded-for']) {
      ip = req.headers['x-forwarded-for'].split(",")[0];
  } else if (req.connection && req.connection.remoteAddress) {
      ip = req.connection.remoteAddress;
  } else {
      ip = req.ip;
  }console.log('client IP is' + ip);
  res.end(`Your IP address is ${ip} and your source port is ${port}.`);
}).listen(3004);
                            
```

會出現如下

```
x-forward for: undefined
socket remote Address: ::ffff:127.0.0.1
req.connection.socket.remoteAddress: undefined
req.ip: undefined
client IP is::ffff:127.0.0.1
```



