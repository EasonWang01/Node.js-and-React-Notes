# Docker 原理

Docker 把一些舊有的 Linux 核心功能包裝了起來，包含以下：

* Namespace:  提供相關資源隔離功能
* Cgroups: 分配相關 CPU 與 RAM 使用量等
* Chroot: 更改執行環境的根目錄，達到檔案存取限制功能
* Veth: 虛擬網路，提供相關網路轉導功能
* UnionFS: 處理相關文件系統

[https://juejin.im/entry/6844903492734156807](https://juejin.im/entry/6844903492734156807)

## LXC \(Linux Container\)

為 docker 前身，目前 Docker 改為使用自行實作的 libContainer

[https://github.com/opencontainers/runc/tree/master/libcontainer](https://github.com/opencontainers/runc/tree/master/libcontainer)

