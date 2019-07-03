# 瀏覽器快取與緩存（Etag 與 If-None-Match）

#### 為何要用：

假設今天server會回傳API給前端頁面，但今天API回傳資料過大，我們想要在前端進行緩存，當資料內容在server有變動時才會讓前端重新呼叫後端。

#### 範例：

這邊會先示範使用 Etag 與 If-None-Match 來進行緩存，首先server會先在回應請求時在 header 附加上etag，etag 通常會是由response內容進行 hash 而成，然後傳給前端，之後前端下次發送相同 API 給 server 時會把該 etag 轉為 If-None-Match 發送給 server，server 會比對是否跟上次 response內容進行 hash 而成的etag 相同，如果相同則返回304 status code。

這個部分 If-None-Match 瀏覽器會自動實作（value自動在下次請求時設為 Etag 的 value），但Etag附加以及If-None-Match 的比對需要自行在 Server實作。

![](/assets/螢幕快照 2019-07-03 下午5.54.47.png)

## 範例

```js
const http = require('http');
const fs = require('fs');
const etag = require('etag');

const server = http.createServer(function (req, res) {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, OPTION, PUT, POST, DELETE');
  res.setHeader('Access-Control-Allow-Headers', '*');
  if (req.url === '/test') {
    fs.readFile("./test.html", function (err, data) {
      res.writeHead(200, { 'Content-Type': 'text/html' });
      res.write(data);
      res.end();
    });
  }

  if (req.url === '/cc') {
    const payload = {
      a: 11112222,
      c: 222
    };
    if (etag(JSON.stringify(payload)) === req.headers["if-none-match"]) {
      res.statusCode = 304;
      res.end(JSON.stringify(payload));
    } else {
      res.setHeader('ETag', etag(JSON.stringify(payload)))
      res.end(JSON.stringify(payload))
    }
  }
});

server.listen(5000); 

console.log('Node.js web server running at port 5000')
```

test.html

```js
<!doctype HTML>
<html>

<head>
  <title>
    Hi
  </title>
  <script>
    fetch('http://localhost:5000/cc')
      .then(data => {
        console.log(data)
      })
  </script>
</head>

<body>
  <h1>
    Hello
  </h1>
</body>

</html>
```

第一次請求：

![](/assets/螢幕快照 2019-07-03 下午5.56.23.png)

第二次請求：

![](/assets/螢幕快照 2019-07-03 下午5.56.34.png)

# 注意事項：

> 1.如果是跨域請求會在request header 顯示 `provisional headers are shown` ，所以會看不到 If-None-Match
>
> 2.有時chrome 304沒顯示可以用firebox試看看。

# 如果是 expressjs server 已經實作了 304 回應![](/assets/螢幕快照 2019-07-03 下午3.30.17.png)

![](/assets/螢幕快照 2019-07-03 下午3.31.07.png)

