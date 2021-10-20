# 網路診斷工具

## MTR

結合了 ping 與 routetrace

> sudo mtr google.com

![](<../.gitbook/assets/螢幕快照 2020-08-17 上午10.03.35.png>)

[http://www.bitwizard.nl/mtr/](http://www.bitwizard.nl/mtr/)

{% embed url="https://blog.gtwang.org/linux/mtr-linux-network-diagnostic-tool/" %}

## TCPDUMP

監聽相關封包

#### TCP localhost&#x20;

```
sudo tcpdump -i lo0
```

> 可先用 ifconfig 看有哪些網路介面

#### UDP

```
tcpdump -n udp port 14550
```

[https://www.tcpdump.org/manpages/tcpdump.1.html](https://www.tcpdump.org/manpages/tcpdump.1.html)

{% embed url="https://askubuntu.com/questions/913393/sniff-udp-packets-on-a-local-port" %}

## NMAP

可以掃描一個主機有開哪些 PORT，或是某個網段有哪些 IP 可連線

{% embed url="https://blog.gtwang.org/linux/nmap-command-examples-tutorials/" %}

## lsof -i

lsof 可以查看系統上各行程所開啟的檔案，加上 -i 可以查找相關網路連線

```
lsof -i tcp:80
```

## nslookup、dig

兩者都可以查詢 DNS records

```
nslookup google.com
dig google.com
```

