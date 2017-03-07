# React webpack 部署

在package.json的script內會寫下類似如下

package.json
```
"build": "NODE_ENV=production API_HOST=http://rdpc.git4u.net:2650/ babel-node client/startProd.js",
```

start.prod
```
import path from 'path';
import webpack from 'webpack';
import _debug from 'debug';

const debug = _debug('Redux-Bolierplate:Build');

debug('Start Build Source...');

webpack({
  devtool: false,
  entry: [
    'babel-polyfill',
    path.join(__dirname, 'entry.js'),
  ],
  output: {
    path: path.join(__dirname, 'build'),
    filename: 'bundle.js',
    publicPath: '/build/',
  },
  plugins: [
    new webpack.DefinePlugin({
      API_HOST: `"${process.env.API_HOST || '/api'}"`,
      API_VERSION: `"${process.env.API_VERSION || '0.0.0'}"`,
      'process.env.NODE_ENV': '"production"',
    }),
    new webpack.optimize.UglifyJsPlugin(),
    new webpack.optimize.DedupePlugin(),
    new webpack.NoErrorsPlugin(),
  ],
  module: {
    loaders: [{
      test: /\.js$/,
      loader: 'babel',
      exclude: /node_modules/,
      include: __dirname,
    }, {
      test: /\.(jpe?g|png|gif|svg)$/i,
      loaders: [
        'file?hash=sha512&digest=hex&name=[hash].[ext]',
        'image-webpack?bypassOnDebug&optimizationLevel=7&interlaced=false',
      ],
      exclude: /node_modules/,
    }, {
      test: /\.css$/,
      loader: 'style-loader!css-loader'
    }],
  },
}, (err) => {
  if (err) {
    debug(err);
  } else {
    console.log('Build Successful!');
    debug('Build Successful!');
  }
});

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

mac預設路徑
```
 /usr/local/etc/nginx/
```

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
以上面這份config為例，我們會在root也就是電腦根目錄，下面的var資料夾下創建www資料夾．

```
/var/www/um1215-webclient/
```

之後裡面放入um1215-webclient資料夾(隨意取名)，然後再把我們剛build產生出的build資料夾放入

>注意事項：build資料夾內要把index.html檔案放入，並且index.html內要有bundle.js的script引入

之後重啟nginx即可

mac
```
sudo nginx -s stop && sudo nginx
```


如果想要網頁重新整理時也找得到route

```
location / {
    try_files $uri $uri/ /index.html;
}
```

靜態文件

```
location ^~ /static/ {
    root /webroot/static/;
}
location ~* \.(gif|jpg|jpeg|png|css|js|ico)$ {
    root /webroot/res/;
}
```


# #Server render 的部署

因為我們要在nginx下再架一個nodejs server

所以第一步先把server side 的code 用到es6的先轉好，才不會出錯


`babel lib -d dist --presets es2015,stage-2 --copy-files`

>如果出問題先sudo npm install babel-cli -g

之後把client的code build一份bundle.js
```
在webpack.config.js同層使用`sudo webpack`
```
記得把hmr等plugin拿掉，然後把server.js用到webpack的也拿掉

之後把bundle.js放到`express.static`的目錄下即可