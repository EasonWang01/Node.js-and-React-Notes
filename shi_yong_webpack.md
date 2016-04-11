# 使用webpack


1.創造如下結構
```
/app
  main.js
  component.js
/build
   index.html
   
webpack.config.js
```
2.npm init

3.npm install webpack --save-dev

4.設定webpack.config.js
```
var path = require('path');


module.exports = {
    entry: path.resolve(__dirname, 'app/main.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
};
```
5.設定app/component.js
```
'use strict';


module.exports = function () {
    var element = document.createElement('h1');

    element.innerHTML = 'Hello world';

    return element;
};
```
6.設定app/main.js
```
'use strict';
var component = require('./component.js');


document.body.appendChild(component());
```
7.在terminal 輸入  webpack，發現多了bundle.js檔案

8.在bundle.js同層加上index.html
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8"/>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```
##如何讓它不要每次改完都要輸入webpack在編譯一次呢?

在package.json加上
```
{
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  }
}
```
最後 npm run dev

http://localhost:8080

##如何不要重新整理網頁呢

webpack.config.js
```
var path = require('path');

module.exports = {
    entry: [
      'webpack/hot/dev-server',
      'webpack-dev-server/client?http://localhost:8080',
      path.resolve(__dirname, 'app/main.js')
    ],
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
};
```
------------------------------

#為了要用 BabelJS 6 with React and ES6，我們要加入 babel-preset-react and babel-preset-es2015 
詳細
https://edwardsamuel.wordpress.com/2015/11/01/react-syntax-error-unexpected-token/

http://jamesknelson.com/using-es6-in-the-browser-with-babel-6-and-webpack/

More about Babel
https://www.npmjs.com/package/babel-loader

##使用
```
npm install babel-loader babel-core babel-preset-es2015 babel-preset-react --save-dev

```
##更改package.json
```
{
  "name": "class1webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  },
  "author": "Eason",
  "license": "ISC",
  "dependencies": {
    "babel-core": "^6.5.2",
    "babel-loader": "^6.2.2",
    "react": "^0.14.7",
    "webpack": "^1.12.13",
    "webpack-dev-server": "^1.14.1"
  }

}

```


webpack.config.js
```
var path = require('path');
var config = {
  entry: path.resolve(__dirname, 'app/main.js'),
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: /\.jsx?$/, // A regexp to test the require path. accepts either js or jsx
      loader: 'babel', // The module to load. "babel" is short for "babel-loader"
       query: {
                  presets: ['es2015','react']
                }

    }]
  }
};

module.exports = config;
```


有三篇完整的教學文章
1.https://christianalfoni.github.io/react-webpack-cookbook/Getting-started.html

2.http://www.christianalfoni.com/articles/2015_10_01_Taking-the-next-step-with-react-and-webpack

3.http://www.christianalfoni.com/articles/2015_04_19_The-ultimate-webpack-setup


##有關plugin

1.`CommonsChunkPlugin`

讓我們共用的module再一開始即載入病進入cache，不要每次重新網頁又重新載入
(缺點為一開始打包時間會長一點，但之後每次重整網頁節省許多速度)

```
var webpack = require('webpack');

module.exports = {
  devtool: 'inline-source-map',
  entry: {
    app:[
    'webpack-hot-middleware/client',
    './client/client.js'
  ],
  vendor:['react','react-dom']
},

  output: {
    path: require("path").resolve("./dist"),
    filename: 'bundle.js',
    publicPath: '/'
  },
  plugins: [
    new webpack.optimize.OccurrenceOrderPlugin(),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin(),
     new webpack.optimize.CommonsChunkPlugin("vendor", "vendor.bundle.js"),
  ],
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'react-hot',
        loader:'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['react', 'es2015', 'react-hmre']
        }
      }
    ]
  }
}
```
之後再index.html加上
```
<script src="vendor.bundle.js"></script>
  <script src="bundle.js"></script>
```
即可


----

http://rhadow.github.io/2015/05/30/webpack-loaders-and-plugins/

#使用ES7

如:解構賦值
```
npm install stage-0
```
webpack.config.js 加上
```
query: {
          presets: ['react', 'es2015','stage-0', 'react-hmre']
        }
```




官方解說
https://webpack.github.io/docs/list-of-plugins.html#occurrenceorderplugin


#多個HTML檔案分別引入不同bundle


4
```
//For many entry points use arrays as a value of entry property:

entry: {
  app: ['./app/main.js', '.lib/index.js'],
  vendors: ['react']
}
```
app and vendors are arrays, so you can put there as many file paths, as you need.

For output case it is so easy that I found it hard to be true :D
```
output: {
  path: staticPath,
  filename: '[name].js'
}
The [name] is taken from entry properties, so if we have app and vendors as properties, we got 2 output files - app.js and vendors.js.
```