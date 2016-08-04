# 使用nginx

可參考此篇安裝

http://blog.hellojcc.tw/2015/12/07/nginx-beginner-tutorial/

在Linux 下的default webpage 路徑為
```
/usr/share/nginx/html
```
nginx的config 檔案路徑

```
/etc/nginx/ 
```

virtual host 路徑
```
/etc/nginx/sites-available
```

使用reverse proxy
```
  location / {
    proxy_pass http://localhost:3000;
  }
```

讓domain.com www.domain.com都導向https

其原理為任何80port會先導向443再轉到他的pass_proxy

而輸入sakatu.com因為寫在443的config內所以會直接導向pass_proxy

![nginx01](https://cloud.githubusercontent.com/assets/11001914/17393482/6b9b7572-5a56-11e6-912b-c3b738300b26.png)

