# WebSocket 相關 Protocal

1. WebSockets 不受 CORS 限制
2. 不一定要在 Browser 使用
3. browser client 的 onmessage 事件只有在收到完整資料後才會觸發 [https://stackoverflow.com/a/13011241/4622645](https://stackoverflow.com/a/13011241/4622645)
4. WebSocker 架構在 HTTP 之上
5. 會發出 HTTP GET request，然後才升級為 WebSocket 
6. 產生 client request，
7. ```text
   GET /chat HTTP/1.1
   Host: example.com:8000
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
   Sec-WebSocket-Version: 13
   ```

8. 產生 server response
9. ```text
   GET /chat HTTP/1.1
   Host: example.com:8000
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
   Sec-WebSocket-Version: 13

   ```

10. 有關 Sec-Websocket-Key，在 client是隨意產生，但 Server 要以下面規則回覆 
11. 將 client 的 Sec-WebSocket-Key 與 magic string: "258EAFA5-E914-47DA-95CA-C5AB0DC85B11" 串接，, 然後再做 SHA-1 hash 之後再進行 base64 encoding
12. Server 最後要兩個 \r\n 來表示 Header 結束

