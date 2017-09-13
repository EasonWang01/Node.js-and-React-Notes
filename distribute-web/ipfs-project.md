# IPFS project

下載

[https://ipfs.io/docs/install/](https://ipfs.io/docs/install/)

Linux  [https://gist.github.com/MiguelBel/b3b5f711aa8d9362afa5f16e4e972461](https://gist.github.com/MiguelBel/b3b5f711aa8d9362afa5f16e4e972461)

```
sudo apt-get update
sudo apt-get install golang-go -y
wget https://dist.ipfs.io/go-ipfs/v0.4.10/go-ipfs_v0.4.10_linux-386.tar.gz
tar xvfz go-ipfs_v0.4.10_linux-386.tar.gz
sudo mv go-ipfs/ipfs /usr/local/bin/ipfs
```

上傳

```
// 檔案
ipfs add <file path>

// 資料夾
ipfs add -r ./
```

下載

```
ipfs get <file hash>
```





# JS-IPFS

js 版本的IPFS

\(目前在windows無法正常使用\)

上傳

```js
const IPFS = require('ipfs')
const fs = require('fs');

const node = new IPFS()
node.on('ready', () => {
  node.files.add({
    path: '',
    content: fs.createReadStream('./sample.txt')
  }, (err, result) => {
    if (err) { throw err }

    console.log('\nAdded file:', result[0].path, result[0].hash)
    // process.exit()
  })
})
      
```

下載

```js
const IPFS = require('ipfs');

const multihashStr  = 'QmT4sfBdFcEiNbRh2FZfyX8jmFBr41qzKLkL75xYXG36aC';

const node  = new IPFS();

node.on('ready', () => {
node.files.get(multihashStr, function (err, stream) {
  stream.on('data', (file) => {
    // write the file's path and contents to standard out
    try {
     console.log(file);
    } catch(e) {
     console.log('錯誤是' + e)
    }
  })
})
});
```





