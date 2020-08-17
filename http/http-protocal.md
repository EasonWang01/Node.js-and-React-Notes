# HTTP Protocal

HTTP/1.1 預設最多可以同時發送 6 個 TCP connection，也就是在瀏覽器頁面上顯示 6 個 network request

HTTP/2 因為是 multiplex 的連線，所以單一 TCP 連線 處理所有請求。

> [https://stackoverflow.com/questions/985431/max-parallel-http-connections-in-a-browser](https://stackoverflow.com/questions/985431/max-parallel-http-connections-in-a-browser)

所以 HTTP/1.1 會出現 domain sharding 的名詞，利用多個 domain 發送請求來解決最大 TCP connection 限制問題。

> [https://developer.mozilla.org/en-US/docs/Glossary/Domain\_sharding](https://developer.mozilla.org/en-US/docs/Glossary/Domain_sharding)

但是你可能會問，為什麼 HTTP/2 單一 TCP 會比 HTTP/1.1 六個 TCP 連線還要快? 多個連線不是 throughput 會更大嗎。原因是這些連線都是虛擬的軟體定義連線，不是實質上的硬體連線，所以電腦能同時處理的請求還是有限制。

> [https://stackoverflow.com/questions/44864273/http-2-0-one-tcp-ip-connections-vs-6-parallel](https://stackoverflow.com/questions/44864273/http-2-0-one-tcp-ip-connections-vs-6-parallel)



