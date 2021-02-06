# HTTPS 封包解密

要解析封包有兩種方式：

1. 有 RSA key
2. 有 client keylog file.

> SSL traffic can only be decrypted if the user has access either to the appropriate RSA key, or a client keylog file.

## 解析 iOS HTTPS traffic

1.先把 iOS 跟 Mac 連線

{% embed url="http://www.gilles-bertrand.com/2016/07/iphoneappwebtrafficcaptureproxymachttpsniffer.html" %}

2.放置 key log

[https://andydavies.me/blog/2019/12/12/capturing-and-decrypting-https-traffic-from-ios-apps/](https://andydavies.me/blog/2019/12/12/capturing-and-decrypting-https-traffic-from-ios-apps/)

