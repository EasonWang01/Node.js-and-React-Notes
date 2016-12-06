# Redis
1.

windows下載
https://github.com/dmajkic/redis/downloads

Linux mac
http://download.redis.io/releases/redis-2.8.17.tar.gz

2.

cd到目錄後執行` redis-server.exe`

3.再開一個terminal一樣cd 到redis路徑
輸入`redis-cli`可產生client端

4.
試著在client端輸入 `set food noodle`後`get food`

# #使用AWS的Redis ElastiCache

>預設只給EC2做內網連線

有兩種可選一種為Redis，一種為Memcached

我們這裡選擇開Redis

>注意：如果是要免費的記得選擇開啟的機器規格
預設的選項是要付費的，FREE TIRE的免費只有t2 micro的Redis

開完後記得先到`ElastiCache dashboard`上方點選Modify，並選擇和你EC2相同之security group，之後回到`EC2 dashboard`把security group加上 6379的PORT

####連線:
ssh到EC2後使用`telnet`

```
telnet test.aq9nab.ng.0001.apne1.cache.amazonaws.com 6379
```



