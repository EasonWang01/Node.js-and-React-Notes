---
description: 伺服器
---

# 圖片伺服器

自己構建一個可以回傳圖片，並且做身份驗證的 static asset server

```javascript
const path = require('path');
const fs = require('fs');

const mime = {
  html: 'text/html',
  txt: 'text/plain',
  css: 'text/css',
  gif: 'image/gif',
  jpg: 'image/jpeg',
  png: 'image/png',
  svg: 'image/svg+xml',
  js: 'application/javascript'
};

const assetFolder = `${__dirname}/../assets/`;
router.get("/", async (req, res) => {
  const tokenID = req.query.tokenID;
  if(!tokenID) {
    return res.json({
      success: false,
      message: "please provide tokenID",
    });
  }
  // 這裡可以增加身份驗證
  // ....
  const fileName = `${tokenID}.png`
  var type = mime[path.extname(fileName).slice(1)] || 'text/plain';
  const filePath = `${assetFolder}${fileName}`;
  if(!fs.existsSync(filePath)){
    return res.json({
      success: false,
      message: `Avatar not found for tokenID ${tokenID}`,
    });
  }
  const s = fs.createReadStream(filePath);
  s.on('open', function () {
      res.set('Content-Type', type);
      s.pipe(res);
  });
  s.on('error', function (err) {
    console.log(err)
      res.set('Content-Type', 'text/plain');
      res.status(404).end('Not found');
  });
})

```

## 上傳圖片到 Server

[https://blog.logrocket.com/multer-nodejs-express-upload-file/](https://blog.logrocket.com/multer-nodejs-express-upload-file/)
