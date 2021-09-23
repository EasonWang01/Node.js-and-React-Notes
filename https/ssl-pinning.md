---
description: >-
  簡單說就是：一般的瀏覽器與手機會信任被機構簽發的證書，因為預設手機就存了一百多個， 所以 root certificate，許多家發的都會信任。
  所以如果有個 proxy server 做 MITM 就很容易讀取封包。而 SSL pinning 將證書或公鑰加密後寫在應用程式，每次發送時都會跟
  server 去比對字串，不相同就拒絕連線。
---

# SSL pinning

{% embed url="https://stackoverflow.com/questions/45699036/what-is-the-difference-between-ssl-pinning-embedded-in-host-and-normal-certifi" %}

{% embed url="https://appmattus.medium.com/android-security-ssl-pinning-1db8acb6621e" %}

## 步驟：

首先要先知道中間人攻擊原理\(man in middle attack\)，wireshark, proxyman 在進行 https 封包破解時都會用到此方法：

```text
1.某網站有用於非對稱加密的公鑰A、私鑰A。
2.瀏覽器向網站服務器請求，服務器把公鑰A明文給傳輸瀏覽器。
3.中間人劫持到公鑰A，保存下來，把數據包中的公鑰A替換成自己偽造的公鑰B（中間人當然也擁有公鑰B對應的私鑰B）
4.瀏覽器生成一個用於對稱加密的密鑰X，用公鑰B（瀏覽器無法得知公鑰被替換了）加密後傳給服務器。
5.中間人劫持後用私鑰B解密得到密鑰X，再用公鑰A加密後傳給服務器。
6.服務器拿到後用私鑰A解密得到密鑰X。
```

此時如果有進行 SSL pinning 的話

SSL pinning: 將服務器的公鑰進行 sha256 雜湊與 base64 encoding 後產生的固定字串寫在應用程式端（app）。

此時中間人在上面第三部發送公鑰時因為中間人的公鑰進行 sha256 雜湊與 base64 encoding 後的字串與寫在應用程式端的不同，此時應用程式隨即會拒絕連線，解決了中間人攻擊。

#### 其他資料

{% embed url="https://github.com/joeferner/node-http-mitm-proxy" %}

[https://zhuanlan.zhihu.com/p/43789231](https://zhuanlan.zhihu.com/p/43789231)

