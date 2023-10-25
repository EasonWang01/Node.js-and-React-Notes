---
description: 常用於物聯網的一種傳輸格式
---

# Coap

範例：

```javascript
const coap = require("coap");


coap.createServer((req, res) => {
  res.end('Hello ' + req.url.split('/')[1] + '\nMessage payload:\n' + req.payload + '\n')
}).listen(() => {
  const req = coap.request('coap://localhost/Matteo')

  const payload = {
      title: 'this is a test payload',
      body: 'containing nothing useful'
  }

  req.write(JSON.stringify(payload))

  req.on('response', (res) => {
      res.pipe(process.stdout)
      res.on('end', () => {
          process.exit(0)
      })
  })

  req.end()
})
```

可以使用 wireshark 查看封包如下

<figure><img src=".gitbook/assets/截圖 2023-10-25 上午10.50.28.png" alt=""><figcaption></figcaption></figure>

從這段數據中，我們可以解析出以下：

1. **網路協議頭部**：

* `02 00 00 00`：這四個字節網絡協議相關
* `45 00`: 這表示 IP 頭的開始，其中 `45` 是 IPv4 的版本和頭部長度。
* `00 75`: 總長度是 117 字節。
* `21 c0`: 識別符。
* `00 00`: 標誌和片偏移。
* `40`: TTL (Time to Live) 值。
* `11`: 使用的協議，這裡的值是 UDP（用戶數據報協議）。
* `00 00`: 頭部校驗和。
* `7f 00 00 01`: 來源 IP 地址，這是 localhost（127.0.0.1）。
* `7f 00 00 01`: 目的地 IP 地址，同樣是 localhost（127.0.0.1）。

2. **UDP 頭部**：

* `cf 46`: 來源端口。
* `16 33`: 目的地端口。
* `00 61`: UDP 長度。
* `fe 74`: UDP 校驗和。

3. **CoAP 消息**：

* `48 01`: CoAP 消息的頭部，其中 `40` 表示 CoAP 的版本，`01` 表示這是一個 GET 請求。
* `49 fc`: 這部分是 CoAP 的令牌。
* `b2`: 這是一個 CoAP 選項，表示 Uri-Path，後面的值是 `Matteo`。
* `ff`: 這是載荷標記，表示後面的是 CoAP 消息的載荷。

4. **CoAP 載荷**：

從 `7b` 開始到封包的結尾，是一個 JSON 格式的載荷：

```json
jsonCopy code{
  "title": "this is a test payload",
  "body": "containing nothing useful"
}
```

這段數據表示一個從 localhost 發送到 localhost 的 UDP 封包，該封包包含一個 CoAP GET 請求到 `/Matteo` 路徑，並帶有 JSON 格式的 payload。

