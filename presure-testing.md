可參考[http://www.epooll.com/archives/768/](http://www.epooll.com/archives/768/)

先增加file operator

```
sudo sh -c "ulimit -n 65535 && exec su $LOGNAME"
```

# 1.使用Webbench

[https://github.com/EZLippi/WebBench](https://github.com/EZLippi/WebBench)

```
wget http://blog.zyan.cc/soft/linux/webbench/webbench-1.5.tar.gz
tar zxvf webbench-1.5.tar.gz
cd webbench-1.5
sudo make &&sudo make install
```

使用

```
webbench -c 5000 -t 120 <url>
```

# 2.使用Apache Benchmark\(ab\)

文件: [https://httpd.apache.org/docs/2.4/programs/ab.html](https://httpd.apache.org/docs/2.4/programs/ab.html)

下載: [https://httpd.apache.org/download.cgi](https://httpd.apache.org/download.cgi)

```
 sudo apt-get install apache2-utils
```

使用:

```
ab -k -n 50000 -c 9000 -r <url>
```

> 打https會ssl handshake failed

如果併發數過高出現error可參考以下調整\(輸入-r\)

[https://superuser.com/questions/323840/apache-bench-test-error-on-os-x-apr-socket-recv-connection-reset-by-peer-54](https://superuser.com/questions/323840/apache-bench-test-error-on-os-x-apr-socket-recv-connection-reset-by-peer-54)

如果用ip當url錯誤，在後面加`/`即可

```
ab -c 19000 -n 220000 -r 114.28.22.129/
```

如出現too many open files可輸入

```
ulimit -n 60000
```

出現Cannot assign requested address

```
sysctl -w net.ipv4.tcp_timestamps=1  
sysctl -w net.ipv4.tcp_tw_recycle=1
```

將請求來源地址更換

EX:

```
ab -k -r -n 600000 -c 20000 -H "Host: yourip.com" www.target.com.tw/
```

# 3.使用hey\(written in Golang\)

優點:可直接打https

先安裝golang [https://www.digitalocean.com/community/tutorials/how-to-install-go-1-6-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-go-1-6-on-ubuntu-16-04)

[https://github.com/rakyll/hey](https://github.com/rakyll/hey)

之後hey會在go資料夾裡面的bin內



# 4.Siege

安裝:

```
apt-get install siege
```

文件:

https://www.joedog.org/siege-manual/

修改config，輸入以下

```
siege.config
```





