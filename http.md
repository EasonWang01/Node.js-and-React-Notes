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