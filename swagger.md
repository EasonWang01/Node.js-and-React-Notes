# \#簡介

swagger有三個服務，editor,codegen,swagger ui

# 1.

其中editor有提供線上的editor或是本機host

[http://editor.swagger.io/\\#/](http://editor.swagger.io/\#/)

線上的缺點在於無法給別人用

本機缺點在於每個人連上後都能修改，且看不到html版的ui

\#解決方法

[https://app.swaggerhub.com](https://app.swaggerhub.com)

上面為swagger得付費服務，也就是可以產生swagger ui 也可codegen也可編輯，加入團隊成員等，且每個人都能看，但只有團隊成員能改



# 2.如果想自己host swagger ui

參考如下網址即可

https://scotch.io/tutorials/document-your-already-existing-apis-with-swagger



# 3.使用swagger editor 並開啟localhost cors

因沒開啟的話，位於遠端網頁的 swagger editor 無法發送測試

```
app.use('*', function(req, res, next) {
	res.header('Access-Control-Allow-Origin', '*');
	res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
	res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
	res.header('Access-Control-Allow-Credentials', 'true');
  next();
 });
```







