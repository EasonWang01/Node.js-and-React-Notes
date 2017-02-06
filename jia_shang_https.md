#使用Let's encrypt

1.先到此網站

https://www.sslforfree.com

(如果沒顯示，換個瀏覽器)

2.之後輸入你的網域名稱

3.點選Manual verfication

4.之後把它給你的兩個檔案下載，之後放到你的server www目錄下

建造一個資料夾`.well-known`然cd進入

再創一個`acme-challenge`

最後放入兩個下載的key


>1.Create a folder in your domain named ".well-known" if it does not already exist
>
2.Create another folder in your domain under ".well-known" named "acme-challenge" if it does not already exist

>3.Upload the downloaded files to the "acme-challenge" folder

ubuntu路徑如下

`/usr/share/nginx/`

之後點擊下面連結測試，如果有下載下來即為成功

之後就可以點下面綠色按鈕，往下一步

(如閒置太久點下一步時會產生錯誤，需重來)

###接著是重點

5.它會給你三個框框，裡面分別為
```
ca_bundle.crt 
private.key 
certificate.crt
```
用nginx為例子

我個人把它們存放再`/usr/share/nginx/sslcrt`，其他路徑也可以，只要後面的nginx default 檔案內設定相同即可

另外要輸入以下指令，將兩個crt合成一個bundle
```
sudo bash -c 'cat certificate.crt ca_bundle.crt >> bundle.crt'
```

我們要在nginx的default設定檔案內設定https


ubuntu路徑如下

`/etc/nginx/sites-available`

會有一個default檔案，用vim等文字編輯器開啟


開啟後如前面沒設定過，會都是藍色的註解

把它更改如下圖(如果說private.key not match ，把ssl_certificate 的bundle.crt改為certificate.crt試試)






之後

```
1.sudo service nginx stop

2.sudo nginx
```
###注意

這裡可能出現一些key或bundle的https錯誤，最常見的是說begin或end之類，記得每個檔案要有
```
-----BEGIN CERTIFICATE-----

....

-----END CERTIFICATE-----
```
這不是註解

##最後在你的網域前加上`https://`即可


參考至:https://free.com.tw/ssl-for-free/


#完整nginx config

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


#附錄：上Node.js裝上https，不使用nginx

範例如下

```
var fs = require('fs');
var https = require('https');
var app = require('express')();
var options = {
   key  : fs.readFileSync('server.key'),
   cert : fs.readFileSync('server.crt')
};

app.get('/', function (req, res) {
   res.send('Hello World!');
});

https.createServer(options, app).listen(3000, function () {
   console.log('Started!');
});
```

記得設定nginx的reverse proxy

參考
https://www.sitepoint.com/how-to-use-ssltls-with-node-js/


#方法2

使用cloudflare

#自動轉址到https
一個是在nginx做轉址
```
rewrite ^/(.*) https://sakatu.com/$1 permanent;
```
一個是在前端頁面做轉址
```
<script type="text/javascript">
    var host = "class.sakatu.com";
    if ((host == window.location.host) && (window.location.protocol != "https:"))
        window.location.protocol = "https";
</script>
```