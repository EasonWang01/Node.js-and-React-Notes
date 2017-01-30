2# Babel

> 前言:1.我們在webpack寫入babel後可使用es6功能，但在entry file外就無法使用es6功能
>
> 2.Node.js的--harmony不包含module功能

1. 所以要在Node.js使用ES6 的export default 須另外寫一個.babelrc檔案

   ```
   {
   "presets": ["es2015"]
   }
   ```

   2.再於使用export default 的parent檔案，使用\(require\)  
   `babel-core/register`


參考至:[http://stackoverflow.com/questions/32346886/unexpected-reserved-word-import-in-node-js](http://stackoverflow.com/questions/32346886/unexpected-reserved-word-import-in-node-js)

## 第二種方法

```
babel-node server.js
```

將會自動轉有關import或相關ES6.7

## 於Production

不建議以上兩種做法，更好的做法餐好以下連結  
[https://medium.com/@Cuadraman/how-to-use-babel-for-production-5b95e7323c2f\#.3yrne4t0a](https://medium.com/@Cuadraman/how-to-use-babel-for-production-5b95e7323c2f#.3yrne4t0a)

因為babel-node或bable-register在runtime build

所以我們要先compile

```
npm install babel-cli -g
```

之後使用

```
babel lib -d dist --presets es2015,stage-2 --copy-files
```

lib為來源要compile  的server 檔案，dist為compile後會產生的資料夾

[https://github.com/babel/example-node-server\\#getting-ready-for-production-use](https://github.com/babel/example-node-server\#getting-ready-for-production-use)

>注意，css檔案不會被compile，所以如果是把整個資料夾compile裡面的css不會出現在compile後的資料夾

http://stackoverflow.com/questions/32642685/babel-cli-copy-nonjs-files



## 有關import 'babel-polyfill';

```
用來將node.js之外的ES6、7語法compile
```

不錯的文章

[http://www.ruanyifeng.com/blog/2016/01/babel.html](http://www.ruanyifeng.com/blog/2016/01/babel.html)

