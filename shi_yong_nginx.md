使用nginx

可參考此篇安裝

[http://blog.hellojcc.tw/2015/12/07/nginx-beginner-tutorial/](http://blog.hellojcc.tw/2015/12/07/nginx-beginner-tutorial/)

重啟指令

```
sudo nginx -s stop && sudo nginx
```

## 在Linux ubuntu 下的default webpage 路徑為

```
var/www/html
```

或

```
/usr/share/nginx/html
```

## nginx的config 檔案路徑

```
/etc/nginx/sites-available/default
```

## 使用reverse proxy

```
location / {
    proxy_pass http://localhost:3000;
  }
```

## 讓domain.com www.domain.com都導向https

可參考下圖配置範例

![sd](https://cloud.githubusercontent.com/assets/11001914/17406653/ed731d6c-5a96-11e6-971a-fabbde3a4a9f.png)

完整範例:

```
server {        
  listen 80;        
  server_name sakatu.com  www.sakatu.com;
  rewrite ^/(.*) https://sakatu.com/$1 permanent;
}
server {        
  server_name sakatu.com;
  listen 443;
  ssl on;
  ssl_certificate  /usr/share/nginx/sslcrt/bundle.crt;        
  ssl_certificate_key /usr/share/nginx/sslcrt/private.key;     
  location / {          
    proxy_pass http://localhost:3000;      
  }}
```

## websocket有問題

加入以下配置

```
location / {
proxy_pass http://localhost:8080;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $host;
}
```

# 注意事項

```
使用AWS或其他VPS記得開防火牆的inbound記得開443PORT
```

設置可參考一篇不錯的文章  
[https://www.linode.com/docs/websites/nginx/how-to-configure-nginx\#start-stop-reload](https://www.linode.com/docs/websites/nginx/how-to-configure-nginx#start-stop-reload)

有關使用cloudflare與nginx配置之SSL可參考web\_Basic之cloudflare章節

# 配置GZIP

[https://www.digitalocean.com/community/tutorials/how-to-add-the-gzip-module-to-nginx-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-add-the-gzip-module-to-nginx-on-ubuntu-14-04)

[http://www.cnblogs.com/zfying/archive/2012/07/07/2580876.html](http://www.cnblogs.com/zfying/archive/2012/07/07/2580876.html)

# 配置Request Limit

[https://www.nginx.com/blog/rate-limiting-nginx/](https://www.nginx.com/blog/rate-limiting-nginx/)

EX:

```
limit_req_zone $binary_remote_addr zone=req_zone:10m rate=5r/s;
server {
        listen 443;
        server_name api.example.com;
        gzip on;
        ssl on;
        ssl_certificate sites-available/c.pem;
        ssl_certificate_key sites-available/c.key;
        underscores_in_headers on;
        location / {
                limit_req zone=req_zone burst=10 nodelay;
                proxy_pass http://localhost:3001;
        }
}
```

## 配置Load Balance

```js
http { 
  upstream cluster { 
      server 127.0.0.1:3000; 
      server 127.0.0.1:3001; 
      server 127.0.0.1:3002; 
      server 127.0.0.1:3003; 
  } 
  server { 
       listen 80; 
       server_name www.domain.com; 
       location / { 
            proxy_pass http://cluster;
       } 
  }
}
```

## 

## 可能錯誤

1.[Nginx fails to stop and nginx.pid is missing](https://serverfault.com/questions/565339/nginx-fails-to-stop-and-nginx-pid-is-missing)

解決方法

```
 ps -ef |grep nginx
   會顯示如下
    www-data 16751     1  0 Jun19 ?        00:00:07 nginx: worker process

 然後

  sudo kill -9 16751


  重啟


  sudo nginx
```

EXAMPLE:

```
server {
 listen 80;
  server_name sakatu.com;
  rewrite ^/(.*) https://rent.sakatu.com/$1 permanent;
}
server {
  listen 80;
  server_name rent.sakatu.com;
  rewrite ^/(.*) https://rent.sakatu.com/$1 permanent;
}
server {
  listen 80;
  server_name www.sakatu.com;
  rewrite ^/(.*) https://sakatu.com permanent;
}


server {
  server_name rent.sakatu.com;
  listen 443;
  root /home/ubuntu/sharehandi_web/build;
  gzip on;
  gzip_vary on;
   gzip_proxied any;
  gzip_comp_level 6;
  gzip_buffers 16 8k;
  gzip_http_version 1.1;
  gzip_min_length 256;
  gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/vnd.ms-fontobject application/x-font-ttf font/opentype image/svg+xml image/x-icon;


  ssl on;
  ssl_certificate  /usr/share/nginx/sslcrt/bundle.crt;
  ssl_certificate_key /usr/share/nginx/sslcrt/private.key;
  location / {
    try_files $uri  /index.html;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
  }
  location /google7d69523ad2a54000.html{
   try_files /../../googlesearch/google7d69523ad2a54000.html /test.html;
  }
    location /sitemap.txt {
   try_files /../../sitemap.txt /test.html;
  }

}


server {
 listen 443;
 server_name chat.sakatu.com;
 location / {
  proxy_pass http://localhost:3005;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  proxy_set_header Host $host;

 }

}
```

# 讀取index.html

```js
server {
  listen 80 default;
  root /home/yichengww/build;
  location / {
    try_files $uri /index.html;
  }
}
```

> 記得root不可寫`~`

# 注意事項

#### 1.有時如果以下指令出現`"nginx.pid" failed`

```
nginx -s reload
```

可替換為

```
sudo service nginx restart
```

### 2.nginx: \[emerg\] open\(\) "/usr/local/Cellar/nginx/1.15.0/logs/error.log" failed \(2: No such file or directory\)

可以創建一個error.log檔案即可

```
mkdir /usr/local/Cellar/nginx/1.15.0/logs && echo > error.log
```

#### 3.nginx: \[emerg\] bind\(\) to 0.0.0.0:82 failed \(13: Permission denied\)

```
使用sudo nginx 即可
```



