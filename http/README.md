# http

## HTTP

```text
var http = require('http');

http.createServer(function (request, response){
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
}).listen(3000);

console.log('Server running on port 3000.');
```

### 純 Node.js 接收 POST request

```javascript
const http = require('http');

const setCORS = () => {
  const headers = {};
  headers["Access-Control-Allow-Origin"] = "*";
  headers["Access-Control-Allow-Methods"] = "POST, GET, PUT, DELETE, OPTIONS";
  headers["Access-Control-Allow-Credentials"] = false;
  headers["Access-Control-Max-Age"] = '86400'; // 24 hours
  headers["Access-Control-Allow-Headers"] = "X-Requested-With, X-HTTP-Method-Override, Content-Type, Accept";
  return headers
}

const parseBody = (req, callback) => {
  let body = '';
  req.on('data', chunk => {
    body += chunk.toString();
  });
  req.on('end', () => {
    callback(body);
  });
}

http.createServer(function (req, res) {
  if (req.method === 'OPTIONS') {
    const headers = setCORS();
    res.writeHead(200, headers);
    res.end();
  }
  if (req.url === '/') {
    if (req.method === "POST") {
      parseBody(req, (body) => {
        console.log(JSON.parse(body));
        const headers = setCORS();
        res.writeHead(200, headers);
        res.end(JSON.stringify({result: 'ok'}));
      })
    }
  }
}).listen(5000);

console.log('Server running on port 5000.');
```

### 

### 包含路由與讀取Body

```javascript
const http = require('http');

const parseBody = (req, callback) => {
  let body = '';
  req.on('data', chunk => {
    body += chunk.toString();
  });
  req.on('end', () => {
    callback(body);
  });
}

http.createServer(function (req, res) {
    if (req.method === 'OPTIONS') {
        var headers = {};
        headers["Access-Control-Allow-Origin"] = "*";
        headers["Access-Control-Allow-Methods"] = "POST, GET, PUT, DELETE, OPTIONS";
        headers["Access-Control-Allow-Credentials"] = false;
        headers["Access-Control-Max-Age"] = '86400'; // 24 hours
        headers["Access-Control-Allow-Headers"] = "X-Requested-With, X-HTTP-Method-Override, Content-Type, Accept";
        res.writeHead(200, headers);
        res.end();
    }
  if (req.url === '/') {
    if (req.method === "POST") {
      parseBody(req, (body) => {
        console.log(JSON.parse(body));
        res.end('ok');
      })
    }
  }
}).listen(3000);

console.log('Server running on port 3000.');
```

### 因為 POST request 會先有一個 options 請求，所以要先回覆

```javascript
    if (req.method === 'OPTIONS') {
        var headers = {};
        headers["Access-Control-Allow-Origin"] = "*";
        headers["Access-Control-Allow-Methods"] = "POST, GET, PUT, DELETE, OPTIONS";
        headers["Access-Control-Allow-Credentials"] = false;
        headers["Access-Control-Max-Age"] = '86400'; // 24 hours
        headers["Access-Control-Allow-Headers"] = "X-Requested-With, X-HTTP-Method-Override, Content-Type, Accept";
        res.writeHead(200, headers);
        res.end();
    }
```

讀檔案

```text
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

```text
將'Content-Type': 'text/plain' 改為'Content-Type': 'text/html'
```

## 寫檔案Request與讀檔案Server

Server.js

```javascript
var express = require('express');
var http = require('http');
var path = require('path');
var fs = require('fs');

var app = express();

app.set('port', process.env.PORT || 3007);

app.post('/upload/:filename', function (req, res) {
  var filename = path.basename(req.params.filename);
  filename = path.resolve(__dirname, filename);
  var dst = fs.createWriteStream(filename);
  req.pipe(dst);
  dst.on('drain', function() {
    console.log('drain', new Date());
    req.resume();
  });
  req.on('end', function () {
    res.send(200);
  });
});

http.createServer(app).listen(app.get('port'), function () {
  console.log('Express server listening on port ' + app.get('port'));
});
```

client.js

```javascript
const path = require('path');
const fs = require('fs');
const http = require('http');

var filepath = __dirname + '/../tt.js'

var rs = fs.createReadStream(filepath);

var options = {
  host: "localhost",
  port: 3007,
  path: '/upload/' + path.basename(filename),
  method: "POST",
  headers: {
    "test": "123",
  }
};
const req = http.request(options, res => {
  console.log(`Status Code: ${res.statusCode}`);
  res.on('data', function (data) {
    console.log(data.toString());
  });
});

req.on('drain', function () {
  console.log('drain', new Date());
  rs.resume();
});

rs.on('end', function () {
  console.log('uploaded finish');
});

req.on('error', function (err) {
  console.error('cannot send file to ' + target + ': ' + err);
});

rs.pipe(req);
```

## 靜態Server

```javascript
var http = require("http"),
    url = require("url"),
    path = require("path"),
    fs = require("fs")
    port = process.argv[2] || 8888;

http.createServer(function(request, response) {

  var uri = url.parse(request.url).pathname
    , filename = path.join(process.cwd(), uri);

  fs.exists(filename, function(exists) {
    if(!exists) {
      response.writeHead(404, {"Content-Type": "text/plain"});
      response.write("404 Not Found\n");
      response.end();
      return;
    }

    if (fs.statSync(filename).isDirectory()) filename += '/index.html';

    fs.readFile(filename, "binary", function(err, file) {
      if(err) {        
        response.writeHead(500, {"Content-Type": "text/plain"});
        response.write(err + "\n");
        response.end();
        return;
      }

      response.writeHead(200);
      response.write(file, "binary");
      response.end();
    });
  });
}).listen(parseInt(port, 10));

console.log("Static file server running at\n  => http://localhost:" + port + "/\nCTRL + C to shutdown");
```

## \#取得remote ip

如果沒有proxy可用req.socket.remoteAddress 但如果有proxy的話req.socket.remoteAddress

用瀏覽器發送請求如果server沒有在nginx的proxy後面會取不到`x-forwarded-for`

> 注意 如果是在proxy後面 例如nginx

```javascript
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

```text
x-forward for: undefined
socket remote Address: ::ffff:127.0.0.1
req.connection.socket.remoteAddress: undefined
req.ip: undefined
client IP is::ffff:127.0.0.1
```

必須在nginx config加上

```text
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

之後即會出現 x-forwarded-for的ip

```text
x-forward for: 122.146.89.8
socket remote Address: ::ffff:127.0.0.1
req.connection.socket.remoteAddress: undefined
req.ip: undefined
client IP is122.146.89.8
```

如果我們把上面的x-forwarded-for請求spoof

改為其他IP

```javascript
const querystring = require('querystring');
const http = require('http');


const options = {
  hostname: '35.190.233.54' ,
  port: 3002,
  path: '/',
  method: 'GET',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
    'X-Forwarded-For':　'133.333.33.33',
  }
};

const req = http.request(options, (res) => {
  console.log(`STATUS: ${res.statusCode}`);
  console.log(`HEADERS: ${JSON.stringify(res.headers)}`);
  res.setEncoding('utf8');
  res.on('data', (chunk) => {
    console.log(`BODY: ${chunk}`);
  });
  res.on('end', () => {
    console.log('No more data in response.');
  });
});

req.on('error', (e) => {
  console.error(`problem with request: ${e.message}`);
});

// write data to request body
req.write(postData);
req.end();
```

之後nginx的x forwarded會出現如下

```text
x-forwarded-for: 133.333.33.33, 122.146.89.8
```

第一個是我們spoof的位置 第二個是原本client的真實ip

也可用如下測試\(spoof一個x-forwarded-for\)

```text
curl http://35.190.233.54:3002/ -H 'X-Forwarded-For: 1.1.1.1'
```

不錯的文章

[https://imququ.com/post/x-forwarded-for-header-in-http.html](https://imququ.com/post/x-forwarded-for-header-in-http.html)

## 寫一個Proxy  Server

接收到請求後可以進行轉發，可用來避開cors

```javascript
const http = require("http");
var https = require("https");

var proxy = http.createServer(function (request, response) {
  response.setHeader('Access-Control-Allow-Origin', '*');
  response.setHeader('Access-Control-Allow-Methods', 'GET, OPTION, PUT, POST, DELETE');
  response.setHeader('Access-Control-Allow-Headers', '*');

  var options = {
    "method": request.method,
    "hostname": "pf1sit1fe.yxunistar.com",
    // "port": null,
    "path": request.url
  };
  var req = https.request(options, function (res) {
    res.pipe(response);
  });
  if(typeof request.headers.cookie !== 'undefined') {
    req.setHeader('Cookie', request.headers.cookie); // 將client的cookie加上
  }
  req.end();
}).listen(8080);
```

## 發送Requst記得加上Header content type

```javascript
"content-type": "application/x-www-form-urlencoded",
```

