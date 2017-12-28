# TCP

網路七層協議  
[https://zh.wikipedia.org/wiki/OSI模型](https://zh.wikipedia.org/wiki/OSI模型)

TCP的特色在於傳輸資料時，會有握手的過程，以確保雙方身份，所以花的時間多一點。

並且TCP只能接受單一連線，要與其他連線時要先斷開先前連線。

而UDP的特色在於傳輸資料時，不需要驗  
證資料，不保證正確性，發送端不知道數據是否會正確接收，所以速度較快速

一般瀏覽網頁時使用的協議是HTTP與HTTPS，其主要是基於TCP，為TCP往上之發展

# Node.js中的TCP

在node.js主要使用`net`這個核心模組來提供TCP的相關功能，

一般主要是在做與硬體溝通時會使用到

具有TCP中的TCP server與 TCP client的兩種類型

TCP server:

```js
const net = require('net');
const server = net.createServer((c) => {
  // 'connection' listener
  console.log('client connected');
  c.on('end', () => {
    console.log('client disconnected');
  });
  c.on('error', (err) => {
      console.log(err)
  })
  c.on('data', (data) => {
      console.log(data)
  })
  c.write('hello\r\n');
  //c.pipe(c); 用來echo任何client送出的message給client
});
server.on('error', (err) => {
  throw err;
});
server.listen(8120, () => {
  console.log('server bound');
});


// 監聽ctrl + c 並且disconnect 來避免TCP server產生ECONNRESET ERROR
process.on('SIGINT', function() {
    console.log("Caught interrupt signal");
    client.end();
});
```

TCP client

```js
const client = net.createConnection({ port: 8120 }, () => {
  //'connect' listener
  console.log('connected to server!');
  client.write('world123!\r\n');
});
client.on('data', (data) => {
  console.log(data.toString());
  //client.end();
});
client.on('end', () => {
  console.log('disconnected from server');
  process.exit();
});

// 監聽ctrl + c 並且disconnect 來避免TCP server產生ECONNRESET ERROR
process.on('SIGINT', function() {
    console.log("Caught interrupt signal");
    client.end();
});
```

同時建立Server與建立Client

```js
const net = require('net');
const server = net.createServer((c) => {
  // 'connection' listener
  console.log('client connected');
  c.on('end', () => {
    console.log('client disconnected');
  });
  c.write('hello\r\n');
  c.pipe(c);
});
server.on('error', (err) => {
  throw err;
});
server.listen(8121, () => {
  console.log('server bound');
});


const client = net.createConnection({ port: 8120 }, () => {
  //'connect' listener
  console.log('connected to server!');
  client.write('world123!\r\n');
});
client.on('data', (data) => {
  console.log(data.toString());
  //client.end();
});
client.on('end', () => {
  console.log('disconnected from server');
  process.exit();
});

// 監聽ctrl + c 並且disconnect 來避免TCP server產生ECONNRESET ERROR
process.on('SIGINT', function() {
    console.log("Caught interrupt signal");
    client.end();
});
```

TCP的createServer的socket實例永遠會保持在最後連線的client

所以想要達到TCP廣播可參考:

[https://gist.github.com/creationix/707146](https://gist.github.com/creationix/707146)

如果想在TCP封包上加密可參考:

[https://nodejs.org/dist/latest-v7.x/docs/api/tls.html\#tls\_class\_tls\_server](https://nodejs.org/dist/latest-v7.x/docs/api/tls.html#tls_class_tls_server)

https://docs.nodejitsu.com/articles/cryptography/how-to-use-the-tls-module/

