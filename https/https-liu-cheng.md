# HTTPS 流程

<figure><img src="../.gitbook/assets/截圖 2024-07-14 上午9.19.05.png" alt=""><figcaption><p><a href="https://blog.bytebytego.com/p/how-does-https-work-episode-6">https://blog.bytebytego.com/p/how-does-https-work-episode-6</a></p></figcaption></figure>

<figure><img src="../.gitbook/assets/截圖 2024-07-13 下午10.39.28.png" alt=""><figcaption></figcaption></figure>

可參考原始碼：[https://github.com/chromium/chromium/blob/main/net/socket/ssl\_client\_socket\_impl.cc#L474](https://github.com/chromium/chromium/blob/main/net/socket/ssl\_client\_socket\_impl.cc#L474)

接著看 Wireshark 實際發送封包之流程

<figure><img src="../.gitbook/assets/截圖 2024-07-13 下午10.48.19.png" alt=""><figcaption></figcaption></figure>

