# HTTPS 流程

<figure><img src="../.gitbook/assets/截圖 2024-07-13 下午10.39.28.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/截圖 2024-07-14 上午9.53.26.png" alt=""><figcaption><p><a href="https://blog.bytebytego.com/p/how-does-https-work-episode-6">https://blog.bytebytego.com/p/how-does-https-work-episode-6</a></p></figcaption></figure>

可參考原始碼：[https://github.com/chromium/chromium/blob/main/net/socket/ssl\_client\_socket\_impl.cc#L474](https://github.com/chromium/chromium/blob/main/net/socket/ssl\_client\_socket\_impl.cc#L474)

接著看 Wireshark 實際發送封包之流程

<figure><img src="../.gitbook/assets/截圖 2024-07-14 上午9.52.23.png" alt=""><figcaption></figcaption></figure>

HTTPS 的交握步驟詳細如下：

* **ClientHello**：
  * 客戶端生成一個隨機數 `random-client`，並將其連同支持的TLS版本、密碼套件（cipher suites）、壓縮方法和可選擴展等信息發送到服務器端。
  * 傳遞內容：
    * **支持的TLS版本**：表示客戶端支持的TLS協議版本（例如TLS 1.2或TLS 1.3）。
    * **密碼套件**：包括客戶端支持的加密算法、鑰匙交換算法、MAC算法等的列表。
    * **擴展**：例如SNI（Server Name Indication）擴展，用於支持多域名的HTTPS。
* **ServerHello**：
  * 服務器生成一個隨機數 `random-server`，並選擇TLS版本、密碼套件，並將其連同服務器的公鑰證書一併回傳給客戶端。
  * 傳遞內容：
    * **公鑰證書**：服務器的數字證書，包含服務器的公鑰和由可信任的證書頒發機構（CA）簽名的證書。
    * **密碼套件選擇**：服務器從客戶端提供的列表中選擇一個共同支持的密碼套件。
* **客戶端回應**：
  * **描述**：客戶端收到服務器的`ServerHello`消息後，保存`random-server`和公鑰，然後生成一個`premaster secret`，並用服務器的公鑰加密後發送給服務器。
  * **技術細節**：
    * **premaster secret**：一個隨機生成的值，用於生成會話密鑰。此值僅在這次會話中使用。
    * **加密**：使用服務器的公鑰加密`premaster secret`，保證傳輸過程中的安全。
* **服務器解密**：
  * **描述**：服務器使用其私鑰解密客戶端傳來的`premaster secret`，此時雙方都擁有了`random-client`、`random-server`和`premaster secret`這三個關鍵要素。
  * **技術細節**：
    * **私鑰解密**：服務器使用其私鑰解密`premaster secret`，保證只有服務器能夠獲取此值。
    * **會話密鑰生成**：雙方使用`random-client`、`random-server`和`premaster secret`通過協商好的算法生成會話密鑰（`session keys`）。
*   **建立安全通道**：

    * **描述**：安全通道建立後，雙方開始使用會話密鑰加密通信內容，保證傳輸的機密性和完整性。
    *   **技術細節**：

        * **Change Cipher Spec**：通知對方接下來的通信將使用新協商的密碼套件。
        * **Finished**：雙方交換加密的`Finished`消息，確認握手過程無誤並開始安全通信。



    > `session key`: 是 TLS/SSL 最後協商得出的共同密鑰，用来進行後續所有封包的對稱式加密。

可將以下檔案導入 Wireshark 查看

{% file src="../.gitbook/assets/https-flow.pcapng" %}
