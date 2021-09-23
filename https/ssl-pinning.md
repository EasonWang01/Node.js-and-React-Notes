---
description: >-
  簡單說就是：一般的 https server 會信任被機構簽發的證書，  所以不管是哪家簽發的都會信任，並且可以解密封包，SSL pinning
  將證書或公鑰加密後寫在應用程式，每次發送時都會跟 server 去比對字串，不相同就拒絕連線。
---

# SSL pinning

{% embed url="https://stackoverflow.com/questions/45699036/what-is-the-difference-between-ssl-pinning-embedded-in-host-and-normal-certifi" %}

[https://appmattus.medium.com/android-security-ssl-pinning-1db8acb6621e](https://appmattus.medium.com/android-security-ssl-pinning-1db8acb6621e)

