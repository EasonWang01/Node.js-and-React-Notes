# ESLint

##用處:用來檢查code的語法
1.安裝

```
npm install -g eslint  
```
2.配置

```
於根目錄新增   .eslintrc.json
```
```
{
  "globals": {
    // Put things like jQuery, etc
    "jQuery": true,
    "$": true
  },
  "env": {
    // I write for browser
    "browser": true,
    // in CommonJS
    "node": true
  },
  // To give you an idea how to override rule options:
  "rules": {
    // Tons of rules you can use, for example...
    "quotes": [1, "double"]
  }
}
```
看到上面的rules的數字，對照下表，以上面為例，意思是對所有出現quotes(引號)的地方要使用double(兩個引號)，強制性為1(只產生警告訊息)
```
0 - Disable the rule
1 - Warn about the rule
2 - Throw error about the rule
```
##其他rules可參考官網
http://eslint.org/docs/rules/

3.使用parser去解析ES6
```
npm install babel-eslint
```
之後在.eslintrc.json加上
```
{
  "parser": "babel-eslint",
  "rules": {
    ...
  }
}
```



