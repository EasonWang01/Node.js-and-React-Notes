---
description: 使用 Wireshark 解密 HTTPS 封包
---

# HTTPS 封包解密

在 Wireshark 要解析封包有兩種方式：

1. 有 RSA key
2. 有 client keylog file.

> SSL traffic can only be decrypted if the user has access either to the appropriate RSA key, or a client keylog file.

## 使用 Key log file 解密 HTTPS 流量

Chrome, Firefox 瀏覽器在 https 連線後可找尋 keylog file 存放相關 tls 解密資訊，所以如下設置

`export SSLKEYLOGFILE=~/.ssl-key.log`

然後必須用同個 terminal 開啟 chrome

輸入：`/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome`

之後再到 Wireshark 設置 key log file path

`Wireshark -> Preference -> Protocols -> TLS -> (Pre) Master secret log filename`

之後原本 Application Data 即會顯示

## 解析 iOS HTTPS traffic

1.先把 iOS 跟 Mac 連線

{% embed url="http://www.gilles-bertrand.com/2016/07/iphoneappwebtrafficcaptureproxymachttpsniffer.html" %}

2.放置 key log

[https://andydavies.me/blog/2019/12/12/capturing-and-decrypting-https-traffic-from-ios-apps/](https://andydavies.me/blog/2019/12/12/capturing-and-decrypting-https-traffic-from-ios-apps/)
