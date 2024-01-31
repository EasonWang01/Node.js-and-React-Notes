---
description: 憑證
---

# AWS HTTPS 憑證

建立憑證要到 Certificate Manager，憑證可用於 ELB、Cloudfront 等等。

此憑證即為常聽見的 HTTPS 憑證。

<figure><img src="../.gitbook/assets/截圖 2024-01-31 下午11.24.04.png" alt=""><figcaption></figcaption></figure>

創建後選擇驗證方式為 DNS，之後到 DNS 紀錄中加入 CNAME，名稱和值對應填上他下方提供的即可，過幾分鐘後幾可驗證成功。

如果更換地區的話，同 domain or subdomain 不需要重新驗證 domain。
