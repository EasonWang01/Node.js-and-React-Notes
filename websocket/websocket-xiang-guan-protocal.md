# WebSocket 相關 Protocal

1. WebSockets 不受 CORS 限制
2. 不一定要在 Browser 使用
3. browser client 的 onmessage 事件只有在收到完整資料後才會觸發 [https://stackoverflow.com/a/13011241/4622645](https://stackoverflow.com/a/13011241/4622645)
4. WebSocker 架構在 HTTP 之上
5. 會發出 HTTP GET request，然後才升級為 WebSocket 

## 建立連線

1. 產生 client request
2. ```text
   GET /chat HTTP/1.1
   Host: example.com:8000
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
   Sec-WebSocket-Version: 13
   ```

3. 產生 server response
4. ```text
   TTP/1.1 101 Switching Protocols
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=

   ```

5. 有關 Sec-Websocket-Key，在 client是隨意產生的 base64 字串 
6. ```text
   client-Sec-Websocket-Key = randomBytes(16).toString('base64')
   ```
7. 將 client 的 Sec-WebSocket-Key 與 magic string: "258EAFA5-E914-47DA-95CA-C5AB0DC85B11" 串接，, 然後再做 SHA-1 hash 之後再進行 base64 encoding
8. Server 最後要兩個 \r\n 來表示 Header 結束

## 發送 Data 

[https://github.com/websockets/ws/blob/master/lib/sender.js](https://github.com/websockets/ws/blob/master/lib/sender.js)

## 持續發送 heartBeat 保持連線

> A ping or pong is just a regular frame, but it's a **control frame**. Pings have an opcode of `0x9`, and pongs have an opcode of `0xA`. When you get a ping, send back a pong with the exact same Payload Data as the ping \(for pings and pongs, the max payload length is 125\). You might also get a pong without ever sending a ping; ignore this if it happens.

## 結束連線

[https://tools.ietf.org/html/rfc6455\#section-5.5.1](https://tools.ietf.org/html/rfc6455#section-5.5.1)

## 參考資料

[https://developer.mozilla.org/en-US/docs/Web/API/WebSockets\_API/Writing\_WebSocket\_servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)

