# TCP

即為 net module

### 簡易的 http server

```javascript
const net = require('net');
const server = net.createServer((socket) => {
  socket.on('data', data => {
    console.log(data.toString())
    socket.end(`
HTTP/1.1 200 OK\r\nContent-type: text/html\r\n
<html>
  <head>
    <style>
      body {
        font-size: 20px
      }
    </style>
  </head>
  <body>test</body>
</html>
    `)
  })
}).on('error', (err) => {
  // Handle errors here.
  throw err;
});

server.listen(8124, () => {
  console.log('opened server on', 8124);
});
```

