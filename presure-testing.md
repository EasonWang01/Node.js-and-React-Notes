可參考http://www.epooll.com/archives/768/

#1.使用Webbench

https://github.com/EZLippi/WebBench
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

#2.使用Apache Benchmark(ab)


```
 sudo apt-get install apache2-utils
```

```
ab -k -n 50000 -c 9000 <url>
```

>打https會ssl handshake failed

#3.使用hey(written in Golang)

優點:可直接打https

先安裝golang https://www.digitalocean.com/community/tutorials/how-to-install-go-1-6-on-ubuntu-16-04

https://github.com/rakyll/hey

之後hey會在go資料夾裡面的bin內
