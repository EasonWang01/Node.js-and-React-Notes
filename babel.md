# Babel

>前言:1.我們在webpack寫入babel後可使用es6功能，但在entry file外就無法使用es6功能
>
>2.Node.js的--harmony不包含module

1. 所以要在Node.js使用ES6 的export default 須寫一個.babelrc檔案
```
{
   "presets": ["es2015"]
}
```
2.再於使用export default 的parent檔案，使用
`babel-core/register`

參考至:http://stackoverflow.com/questions/32346886/unexpected-reserved-word-import-in-node-js


