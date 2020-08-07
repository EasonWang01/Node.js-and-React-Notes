# DNS

## DNS 解析

1.我們可以在電腦更改 `/etc/hosts`直接指定哪個 domain 要對應哪個 IP。

2.但假設沒有指定的話就會到 `/etc/resolv.conf` 尋找裡面寫的 DNS server 做查詢。

3.目前台灣大多是 Hinet 中華電信的 DNS server `168.95.1.1` 他會再去查詢

4.假設找不到的話他會去找 **root name server \( 被** ICANN 旗下的 IANA 管轄**，全世界共有 13 個 root name server \)**

> [https://zh.wikipedia.org/wiki/%E6%A0%B9%E7%B6%B2%E5%9F%9F%E5%90%8D%E7%A8%B1%E4%BC%BA%E6%9C%8D%E5%99%A8](https://zh.wikipedia.org/wiki/%E6%A0%B9%E7%B6%B2%E5%9F%9F%E5%90%8D%E7%A8%B1%E4%BC%BA%E6%9C%8D%E5%99%A8)
>
> [https://www.iana.org/domains/root/servers](https://www.iana.org/domains/root/servers)

5.如果是 .tw 之類的在 root dns server 沒記錄，會再往下找，可以輸入以下指令試看看

```
dig +trace https://www.webnode.tw/
```

## DNS Server 種類

1. Root Name server
2. TLDs Server
3. Authoritative name server

## Resource Record \(RR\)

例如 NS; A; AAAA; PTR; CNAME; MX. SOA等。

## **DNSSEC**

類似於 IPSec 都是用來增強相關安全性。

{% embed url="http://www.cc.ntu.edu.tw/chinese/epaper/0022/20120920\_2206.html" %}

## NS vs SOA

NS record 通常會有多個 DNS server 用來表示有哪些 DNS Server 可供查詢，SOA record 通常只有一個，指向 Master DNS Server，可讓其他 Slave DNS Server Sync 資料，並且 SOA record 後面會跟著一些數字代表 Master 與 Slave 的資料同步策略。

> NS records are used to redirect DNS resolver to the next DNS server that is hosting the next level zone. And, SOA record is used by cluster of DNS servers to sync latest changes from the master to secondary servers
>
> 可參考：
>
> [http://dns-learning.twnic.net.tw/bind/intro6.html\#bb](http://dns-learning.twnic.net.tw/bind/intro6.html#bb)

## BIND

為一套開放原始碼 DNS 軟體，現行的 DNS Server 大家都在用，實作了 TSIG 與 rndc 等功能。

{% embed url="https://zh.wikipedia.org/wiki/BIND" %}

{% embed url="https://gitlab.isc.org/isc-projects/bind9" %}

## TSIG、RNDC

[http://dns-learning.twnic.net.tw/bind/security.html](http://dns-learning.twnic.net.tw/bind/security.html)

