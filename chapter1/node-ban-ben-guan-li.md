---
description: 版本管理
---

# Node 版本管理

以前可以使用 NVM, N 等工具來切換 Node.js 版本，現在推薦使用 Volta，可以在不同專案設定不同版本。

{% embed url="https://docs.volta.sh/guide/getting-started" %}

輸入以下後會自動安裝，並設置指令到環境

```
curl https://get.volta.sh | bash
```

到專案資料夾下輸入

```
volta pin node@v11.14
```

專案內 package.json 會新增

<figure><img src="../.gitbook/assets/截圖 2024-05-16 上午11.05.17.png" alt=""><figcaption></figcaption></figure>

之後在此專案內即為 11.14 版本，外部還是原本安裝的版本。
