# 瀏覽器快取與緩存

#### 為何要用：

假設今天server會回傳API給前端頁面，但今天API回傳資料過大，我們想要在前端進行緩存，當資料內容在server有變動時才會讓前端重新呼叫後端。

#### 範例：

這邊會先示範使用 Etag 與 If-None-Match 來進行緩存，首先server會先在回應請求時在 header 附加上etag，etag 通常會是由response內容進行 hash 而成，然後傳給前端，之後前端下次發送相同 API 給 server 時會把該 etag 轉為 If-None-Match 發送給 server，server 會比對是否跟上次 response內容進行 hash 而成的etag 相同，如果相同則返回304 status code。



