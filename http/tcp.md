# TCP

## TCP

網路七層協議  
[https://zh.wikipedia.org/wiki/OSI模型](https://zh.wikipedia.org/wiki/OSI模型)

TCP的特色在於傳輸資料時，會有握手的過程，以確保雙方身份，所以花的時間多一點。

TCP可以接受多個連接，每個連接會對應到file descriptors。

可參考：[https://stackoverflow.com/a/27182584](https://stackoverflow.com/a/27182584)

而UDP的特色在於傳輸資料時，不需要驗  
證資料，不保證正確性，發送端不知道數據是否會正確接收，所以速度較快速

一般瀏覽網頁時使用的協議是HTTP與HTTPS，其主要是基於TCP，為TCP往上之發展

## Node.js中的TCP

在node.js主要使用`net`這個核心模組來提供TCP的相關功能，

一般主要是在做與硬體溝通時會使用到

具有TCP中的TCP server與 TCP client的兩種類型

> 1.Node.js 的 TCP可以使用以下來取得目前連線中的連接
>
> [https://nodejs.org/api/net.html\#net\_server\_getconnections\_callback](https://nodejs.org/api/net.html#net_server_getconnections_callback)
>
> ```javascript
> server.getConnections((err, count) => {
>   console.log(count)
> })
> ```
>
> 2.net.createServer\(\(socket\) =&gt; ....\)
>
> 雖然TCP連線後會繼續保持，但其中的socket callback會是最後一個連線到server的client，所以需要自行把先前連線的socket保存著

TCP server:

```javascript
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

```javascript
const net = require('net');
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

```javascript
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

### 1.TCP廣播可參考:

[https://gist.github.com/creationix/707146](https://gist.github.com/creationix/707146)

### 2.如果想在TCP封包上加密可參考:

[https://nodejs.org/dist/latest-v7.x/docs/api/tls.html\#tls\_class\_tls\_server](https://nodejs.org/dist/latest-v7.x/docs/api/tls.html#tls_class_tls_server)

[https://docs.nodejitsu.com/articles/cryptography/how-to-use-the-tls-module/](https://docs.nodejitsu.com/articles/cryptography/how-to-use-the-tls-module/)

> 每個tcp建立連線後可以把socket物件記住，之後連線其他的節點後可以再次存取原先socket即可再次傳送訊息。

### 3.限制最大連線數

> ```javascript
> 1. server.maxConnections
>
> 2.設定Linux中的tcp_max_syn_backlog 與 somaxconn
>
> 3.設定listen參數中的backlog
> ```

