# IPFS project

下載

[https://ipfs.io/docs/install/](https://ipfs.io/docs/install/)

Linux  https://gist.github.com/MiguelBel/b3b5f711aa8d9362afa5f16e4e972461

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



