---
description: Email 服務，可以用來綁定 domain 並發送與接收郵件
---

# SendGrid

> 因為目前 mailgun 免費版只有試用期間，之後都要付費，所以如果只是要測試暫時只能選擇 sendgrid

設置完後要去域名服務那邊設置三個 CNAME ![](<../.gitbook/assets/截圖 2023-11-29 下午6.34.52.png>)

## 發送郵件範例

```javascript
const sgMail = require('@sendgrid/mail')
sgMail.setApiKey("...")
const msg = {
  to: '', // Change to your recipient
  from: '', // Change to your verified sender
  subject: '',
  text: '',
  html: '<strong>Node.js</strong>',
}
sgMail
  .send(msg)
  .then(() => {
    console.log('Email sent')
  })
  .catch((error) => {
    console.error(error)
  })
```

## 接收郵件範例

SendGrid 只能串接 server 使用 webhook 來接收郵件。

新增兩個 ![](<../.gitbook/assets/截圖 2023-11-29 下午6.35.59.png>)

然候新增 inbound url:![](<../.gitbook/assets/截圖 2023-11-29 下午6.37.09.png>)

之後寄信到 ...@parse.domain.com 都會發送 POST (multipart/form-data) 到上面的 URL

{% embed url="https://docs.sendgrid.com/for-developers/parsing-email/setting-up-the-inbound-parse-webhook" %}

#### Node.js inbound email 接收範例

> 記得要寄信到 mx 設置的 subdomain

> 因為是 multipart/form-data 所以用 busboy 模組解析

```javascript
const express = require("express");
const app = express();
const busboy = require("busboy");
app.use(express.json());

// GET route
app.get("/", (req, res) => {
  res.send("Test OK");
});

// POST route
app.post("/parse", (req, res) => {
  console.log("POST request");
  const bb = busboy({ headers: req.headers });
  bb.on("file", (name, file, info) => {
    const { filename, encoding, mimeType } = info;
    console.log(
      `File [${name}]: filename: %j, encoding: %j, mimeType: %j`,
      filename,
      encoding,
      mimeType
    );
    file
      .on("data", (data) => {
        console.log(`File [${name}] got ${data.length} bytes`);
      })
      .on("close", () => {
        console.log(`File [${name}] done`);
      });
  });
  bb.on("field", (name, val, info) => {
    console.log(`Field [${name}]: value: %j`, val);
    if (name === "html") {
      console.log("---------------");

      console.log("content:", val);
      console.log("---------------");
    }
  });
  bb.on("close", () => {
    console.log("Done parsing form!");
    res.writeHead(303, { Connection: "close", Location: "/" });
    res.end();
  });
  req.pipe(bb);
});

// Start the server
const PORT = 3001;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```
