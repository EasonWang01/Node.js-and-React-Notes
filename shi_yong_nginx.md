# 使用nginx

可參考此篇安裝

http://blog.hellojcc.tw/2015/12/07/nginx-beginner-tutorial/

##在Linux 下的default webpage 路徑為
```
/usr/share/nginx/html
```
##nginx的config 檔案路徑

```
/etc/nginx/ 
```

##virtual host 路徑
```
/etc/nginx/sites-available
```

##使用reverse proxy
```
  location / {
    proxy_pass http://localhost:3000;
  }
```

##讓domain.com www.domain.com都導向https

可參考下圖配置範例


![sd](https://cloud.githubusercontent.com/assets/11001914/17406653/ed731d6c-5a96-11e6-971a-fabbde3a4a9f.png)


