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

------------------------------

#In order to use BabelJS 6 with React and ES6, we need to use babel-preset-react and babel-preset-es2015 
詳細
https://edwardsamuel.wordpress.com/2015/11/01/react-syntax-error-unexpected-token/

http://jamesknelson.com/using-es6-in-the-browser-with-babel-6-and-webpack/

More about Babel
https://www.npmjs.com/package/babel-loader
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