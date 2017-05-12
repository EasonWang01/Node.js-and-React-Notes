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

使用

```
ab -k -n 50000 -c 9000 <url>
```