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

[https://scotch.io/tutorials/document-your-already-existing-apis-with-swagger](https://scotch.io/tutorials/document-your-already-existing-apis-with-swagger)

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

將上面這段加到code的最上面即可

# ＃工作流程

1.一般會使用如上方式寫nodejs api然後寫swagger editor的規格文件yaml然後貼到你自己host的swagger ui的yaml上

要接api的人即可去你的host網址看

2.或是使用swaggerhub直接線上編輯並可產生只可看的swagger ui


# #開始編寫yaml語言

先到http://editor.swagger.io/#/

可以看到範例，接著我們把它清空，開始編寫自己的版本

最基本的型態
```
swagger: '2.0'

info:
  version: 1.0.0
  title: test API
  description: this is test

schemes:
  - http
host: localhost:3000
basePath: /

paths: 
  
```
其中最上面指的是使用swagger的版本，目前固定是2.0.0

中間部分等於是指定API server的位置，意思為http:localhost:3000/

再來我們會開始往paths裡面寫api

#### #寫paths的步驟

1.路徑 `/test`

2.動詞 `get,post...`

3.四劍客 `summary description parameters responses`

4.在parameters下寫參數
```
 -  name: pageSize
   in: query
   description: Number of persons returned
   type: integer
```

5.在responses下寫response code  `200,304...`

6.每個response code下有  