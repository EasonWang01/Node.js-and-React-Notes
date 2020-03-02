# Dat project

上傳

```text
cd folder
dat share
```

下載

> 記得share的節點要開著

```text
dat clone <hash>
```

```javascript
var Dat = require('dat-node')
// 記住檔案要是資料夾
Dat('../../yi.w/Desktop/aa/', function (err, dat) {
  if (err) throw err

  // 2. Import the files
  dat.importFiles()

  // 3. Share the files on the network!
  dat.joinNetwork()
  // (And share the link)
  console.log('My Dat link is: dat://', dat.key.toString('hex'))
})
```

網站查看檔案

```text
https://datproject.org/dat://c008fb2b2889b99e4512b27677f31c2cf0fcdad07ecfecfa1b157b216a2341a7
```

