# 開始Node

mkdir class1

npm init

發現資料夾多了一個package.json


## 

**注意:不要將主目錄名稱的開頭取名為和任何你要安裝的package名稱相同，安裝時會出問題**

## 


#使用scripts標籤下加入名稱，可使用npm run +名稱

package.json
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
    "webpack": "^1.12.13",
    "webpack-dev-server": "^1.14.1"
  }
}





```

#In order to use BabelJS 6 with React and ES6, we need to use babel-preset-react and babel-preset-es2015 
詳細
https://edwardsamuel.wordpress.com/2015/11/01/react-syntax-error-unexpected-token/

http://jamesknelson.com/using-es6-in-the-browser-with-babel-6-and-webpack/
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