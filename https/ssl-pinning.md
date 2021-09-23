---
description: >-
  簡單說就是：一般的瀏覽器與手機會信任被機構簽發的證書，因為預設手機就存了一百多個， 所以 root certificate，許多家發的都會信任。
  所以如果有個 proxy server 做 MITM 就很容易讀取封包。而 SSL pinning 將證書或公鑰加密後寫在應用程式，每次發送時都會跟
  server 去比對字串，不相同就拒絕連線。
---

# SSL pinning

{% embed url="https://stackoverflow.com/questions/45699036/what-is-the-difference-between-ssl-pinning-embedded-in-host-and-normal-certifi" %}

[https://appmattus.medium.com/android-security-ssl-pinning-1db8acb6621e](https://appmattus.medium.com/android-security-ssl-pinning-1db8acb6621e)

