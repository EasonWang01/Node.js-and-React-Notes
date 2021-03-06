# HTTP/2

## Binary 傳輸

不同於 HTTP/1.1使用 ASCII 傳輸，HTTP/2使用二進位型態傳輸

[https://stackoverflow.com/questions/818122/which-encoding-is-used-by-the-http-protocol](https://stackoverflow.com/questions/818122/which-encoding-is-used-by-the-http-protocol)

並且封包分割為 header frame 和 data frame

[https://developers.google.com/web/fundamentals/performance/http2\#binary\_framing\_layer](https://developers.google.com/web/fundamentals/performance/http2#binary_framing_layer)

## Multiplexing

可透過單一 TCP 連線來發送多個請求，減少 HTTP層的 Head-of-Line blocking \(後面的請求被前面阻塞的請求所擋住\)，不過 HTTP/2 Head-of-Line blocking 還是出現在 TCP層。

也同時可以減少 TCP 連線建立所需要的額外 RTT \(round trip time\)。

> RTT: [https://www.cloudflare.com/learning/cdn/glossary/round-trip-time-rtt/](https://www.cloudflare.com/learning/cdn/glossary/round-trip-time-rtt/)

## Header 壓縮

使用 HPACK 演算法來壓縮。

[https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/)

## Server Push

預先推送靜態資源給前端使用，類似於原先瀏覽器的 &lt;Link preload&gt;。

> * The syntax various slightly between Preload and Push. You can use the Preload directive to initiate a push however this depends on the capabilities of your server/CDN. If you want to explicitly Preload an asset and not Push it you can use `nopush` like so:
>
>   ```text
>   Link: rel=preload; </app/script.js>;  as=script; nopush
>   ```
>
> * You can Push assets as soon as the server receives the initial request from the browser. However, you can only Preload once the browser has received and parsed the HTML file.
> * You can Preload assets from third party domains whereas you can only Push assets from your own domains.
> * The browser support for Preload isn't perfect yet. Firefox, IE, and Edge are all browser that either don't support or only partially support the Preload mechanism. Push however has more support as it is an [HTTP/2](https://www.keycdn.com/blog/keycdn-http2-support) feature which is now the latest version of the HTTP protocol for the Internet. See the current browser support statistics for Preload [here](https://caniuse.com/#search=preload) and for Push [here](https://caniuse.com/#feat=http2).
> * Preload allows you to better define prioritization with the `as` attribute whereas the responsibility of prioritization with Push is shared among the client and server.

[https://www.keycdn.com/blog/http-preload-vs-http2-push](https://www.keycdn.com/blog/http-preload-vs-http2-push)

