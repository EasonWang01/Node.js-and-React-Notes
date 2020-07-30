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

## IPC server

> 通常會指定 /tmp

```javascript
const net = require('net');
const path = require('path');
const server = net.createServer((socket) => {
  socket.on('data', data => {
    console.log(data.toString())
    socket.end('hi')
  })
}).on('error', (err) => {
  // Handle errors here.
  throw err;
});

const port = '/tmp/unix.sock3';
// Grab an arbitrary unused port.
server.listen(port, () => {
  console.log('opened server on', port);
});
```

client

```javascript
const net = require('net');
const port = '/tmp/unix.sock3';
const client = net.createConnection(port, () => {
  // 'connect' listener.
  console.log('connected to server!');
  client.write('world!\r\n');
});
client.on('data', (data) => {
  console.log(data.toString());
  client.end();
});
client.on('end', () => {
  console.log('disconnected from server');
});
```

