# Redis

## Redis

1\.

windows下載 [https://github.com/MSOpenTech/redis/releases](https://github.com/MSOpenTech/redis/releases) [https://github.com/dmajkic/redis/downloads](https://github.com/dmajkic/redis/downloads) (下載後點選redis-server.exe 之後點選redis-cli)

Linux

```
sudo apt install redis-server
```

Mac using homebrew

[https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298#.su9r2yd8u](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298#.su9r2yd8u)

2\.

cd到目錄後執行`redis-server.exe`

3.再開一個terminal一樣cd 到redis路徑 輸入`redis-cli`可產生client端

4\. 試著在client端輸入 `set food noodle`後`get food`

## #使用

基本上使用set 與get 即可用來存儲js相關字串或物件及array，但記得要先轉為string，記得要加上第三個參數callback不然可能無法使用

另外操作的過程不用像mongodb一樣寫在ready裡面

```
const redis = require("redis");
const Redisclient = redis.createClient();

export default () => {

  Redisclient.on("ready", function (err) {
      console.log("Ready");
  });

  Redisclient.on("error", function (err) {
      console.log("Error " + err);
  });
}

exports.Redisclient = Redisclient;
```

```
import { Redisclient } from './redis';
import Redis from './redis';
Redis();

const payload = [{a:12,b:13},{a:12,b:13},{a:12,b:13}];

Redisclient.set("short", JSON.stringify(payload), () => {

});
Redisclient.get("short",function (err, reply) {
    console.log(JSON.parse(reply)); // Will print `OK`
});
```

```
# #使用AWS的Redis ElastiCache

>預設只給EC2做內網連線

有兩種可選一種為Redis，一種為Memcached

我們這裡選擇開Redis

>注意：如果是要免費的記得選擇開啟的機器規格
預設的選項是要付費的，FREE TIRE的免費只有t2 micro的Redis

開完後記得先到`ElastiCache dashboard`上方點選Modify，並選擇和你EC2相同之security group，之後回到`EC2 dashboard`把security group的Inbound加上 6379的PORT

####連線:
ssh到EC2後使用 `telnet`
telnet test.aq9nab.ng.0001.apne1.cache.amazonaws.com 6379
```

```
#開放從電腦本機連線到AWS Redis

利用轉傳規則 ssh -f -N -L6379::6379
```

之後即可本機端和EC2共用同一個Redis 伺服器

再輸入`redis-cli`即可使用

[http://stackoverflow.com/questions/21917661/can-you-connect-to-amazon-elasticache-redis-outside-of-amazon](http://stackoverflow.com/questions/21917661/can-you-connect-to-amazon-elasticache-redis-outside-of-amazon)

## 可用資料結構

```
String (SET GET)
Hash (HSET HGET HDEL HGETALL)
List (LPUSH LPOP RPUSH RPOP)
Set (SADD SPOP)
Sorted Set (ZADD ZRANGE)
```
