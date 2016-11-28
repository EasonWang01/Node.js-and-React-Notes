# React webpack 部署

在package.json的script內會寫下類似如下

```
"build": "NODE_ENV=production API_HOST=http://rdpc.git4u.net:2650/ babel-node client/startProd.js",
```

startProd即為webpack會打包成bundle.js的指令檔案

在此案例中我們在檔案內寫output為
```
  output: {
    path: path.join(__dirname, 'build'),
    filename: 'bundle.js',
    publicPath: '/build/',
  },
```
所以專案build完會產生一個build資料夾，裡面會有buldle.js

##之後開始設定nginx環境


安裝好nginx後我們先去改他的config

ex:

```

http {
server {
  listen 8081 default_server;
  server_name _;
  root /var/www/um1215-webclient/build;
  index index.htm index.html;
  access_log /var/log/um1215-webclient.access.log;
  error_log /var/log/um1215-webclient.error.log;
  client_max_body_size 500m;

 location ~ ^/build {
    root /var/www/um1215-webclient;
  }

  location / {
    try_files $uri /$uri /index.html;
  }
}
include servers/*;
}

```
以上面這份config為例，我們會在root也就是腦根目錄，下面的var資料夾下創建www資料夾．

之後裡面放入um1215-webclient資料夾(隨意取名)，然後再把我們剛build產生出的build資料夾放入

>注意事項：build資料夾內要把index.html檔案放入，並且index.html要有bundle.js的script引入
