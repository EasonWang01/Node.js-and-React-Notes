# swagger

## 簡介

swagger有三個服務，editor,codegen,swagger ui

> 以下範例為Open API 2.0 現在最新為Open API 3.0

## 1.

其中editor有提供線上的editor或是本機host

[http://editor.swagger.io/\\#/](http://editor.swagger.io/#/)

線上的缺點在於無法給別人用

本機缺點在於每個人連上後都能修改，且看不到html版的ui

\#解決方法

[https://app.swaggerhub.com](https://app.swaggerhub.com)

上面為swagger得付費服務，也就是可以產生swagger ui 也可codegen也可編輯，加入團隊成員等，且每個人都能看，但只有團隊成員能改

## 2.如果想自己host swagger ui

參考如下網址即可

[https://scotch.io/tutorials/document-your-already-existing-apis-with-swagger](https://scotch.io/tutorials/document-your-already-existing-apis-with-swagger)

```
1.下載並且安裝node.js 
2. npm install -g http-server 
3. 下載項目https://github.com/swagger-api/swagger-ui 並且解壓。 
4. 進入swagger-ui文件夾。運行http-server命令。 
5. 進入http://127.0.0.1:8080/dist/就可以看到swagger頁面了
```

> 讀取不同的swagger json 只要加上 ?url 即可

```
http://127.0.0.1:8080/dist/?url=https://raw.githubusercontent.com/OAI/OpenAPI-Specification/master/examples/v2.0/json/api-with-examples.json
```

[https://swagger.io/docs/open-source-tools/swagger-ui/usage/installation/](https://swagger.io/docs/open-source-tools/swagger-ui/usage/installation/)

## 3.使用swagger editor 並開啟localhost cors

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

## ＃工作流程

1.一般會使用如上方式寫nodejs api然後寫swagger editor的規格文件yaml然後貼到你自己host的swagger ui的yaml上

要接api的人即可去你的host網址看

2.或是使用swaggerhub直接線上編輯並可產生只可看的swagger ui

## #開始編寫yaml語言

> yaml子項目會後退空兩格，陣列使用`-`表示

先到[http://editor.swagger.io/#/](http://editor.swagger.io/#/)

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

**#寫paths的步驟**

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

5.在responses下寫response code `200,304...`

6.每個response code下有回傳的東西

```
description: A Person
schema:
   required:
     - username
         properties:
           firstName:
             type: string
           lastName:
             type: string
           username:
             type: string
```

## 基本上我們寫這樣就夠了

GET 簡單範本

```
  /getUser:
    # This is a HTTP operation
    get:
      description: |
        取得使用者，使用query方法
      parameters:
        - name: id
          in: query
          description: 使用者id
          required: true
          type: string       
      responses:
        "200":
          description: Success
```

> 在express中使用req.params取得url中使[http://localhost:3000/getUser/123](http://localhost:3000/getUser/123)
>
> 使用req.query取得[http://localhost:3000/getUser?id=123](http://localhost:3000/getUser?id=123)

GET 方法的完整範例

server.js

```
var express = require('express')
var app = express()

app.use('*', function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
    res.header('Access-Control-Allow-Credentials', 'true');
    next();
});

app.get('/allArticle', function (req, res) {
  res.json({
    result: 'ok',
    data: 'thisisall'
  })
})

app.get('/getArticle/:id', function (req, res) {
  console.log(req.params);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.get('/getUser', function (req, res) {
  console.log(req.query);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

yaml

```
swagger: '2.0'

host: localhost:3000

info:
  version: "1.0.0"
  title: <testAPI>

paths:

  /allArticle:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得所有文章
      responses:
        "200":
          description: Success

  /getArticle/{id}:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得特定文章使用params方法
      # This is array of GET operation parameters:
      parameters:
        - name: id
          in: path
          description: 文章id
          required: true
          type: string       
      responses:
        "200":
          description: Success
  /getUser:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得使用者，使用query方法
      # This is array of GET operation parameters:
      parameters:
        - name: id
          in: query
          description: 使用者id
          required: true
          type: string       
      responses:
        "200":
          description: Success
```

Post

> 你可能看過$ref: '#/definitions/test1'，之後把test1另外定義，但個人感覺此種寫法比較分散，於是此處不用此種寫法

## 注意:修改yaml後記得把`Try this operation`重新開啟才會更新

先定義post type

```
consumes:
  - application/x-www-form-urlencoded
```

或是

```
consumes:
  - multipart/form-data
```

路由

```
  /register:         

    post:
      description: add a new user
      # movie info to be stored
      consumes:
        - application/x-www-form-urlencoded
      parameters:
        - name: account
          description: 使用者帳號
          type: string
          in: formData
          required: true
        - name: password
          description: 使用者密碼
          type: string
          in: formData
          required: true
      responses:
        "200":
          description: Success
```

server.js

```
var express = require('express');
var app = express();
var bodyParser = require('body-parser')

app.use(bodyParser());

app.use('*', function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
    res.header('Access-Control-Allow-Credentials', 'true');
    next();
});

app.get('/allArticle', function (req, res) {
  res.json({
    result: 'ok',
    data: 'thisisall'
  })
})

app.get('/getArticle/:id', function (req, res) {
  console.log(req.params);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.get('/getUser', function (req, res) {
  console.log(req.query);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.post('/register',function (req,res) {
  console.log(req.body);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

最後加上PUT與DELETE方法

server.js

```
var express = require('express');
var app = express();
var bodyParser = require('body-parser')

app.use(bodyParser());

app.use('*', function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET, PUT, POST, DELETE, OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Accept, Origin, Content-Type');
    res.header('Access-Control-Allow-Credentials', 'true');
    next();
});

app.get('/allArticle', function (req, res) {
  res.json({
    result: 'ok',
    data: 'thisisall'
  })
})

app.get('/getArticle/:id', function (req, res) {
  console.log(req.params);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.get('/getUser', function (req, res) {
  console.log(req.query);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.post('/register',function (req,res) {
  console.log(req.body);
  res.json({
    result: 'ok',
    data: ['data1','data2']
  })
})

app.put('/user', function (req, res) {
  console.log(req.body);  
  console.log('user info updated!')
  res.send('Got a PUT request at /user');
});

app.delete('/user', function (req, res) {
  console.log(req.body);  
  console.log('user deleted!')
  res.send('Got a DELETE request at /user');
});



app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

yaml

```
swagger: '2.0'

host: localhost:3000
# This is your document metadata
info:
  version: "1.0.0"
  title: <testAPI>

# Describe your paths here
paths:

  /allArticle:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得所有文章
      responses:
        "200":
          description: Success

  /getArticle/{id}:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得特定文章使用params方法
      # This is array of GET operation parameters:
      parameters:
        - name: id
          in: path
          description: 文章id
          required: true
          type: string       
      responses:
        "200":
          description: Success
  /getUser:
    # This is a HTTP operation
    get:
      # Describe this verb here. Note: you can use markdown
      description: |
        取得使用者，使用query方法
      # This is array of GET operation parameters:
      parameters:
        - name: id
          in: query
          description: 使用者id
          required: true
          type: string       
      responses:
        "200":
          description: Success  
  /register:         

    post:
      description: add a new user
      # movie info to be stored
      consumes:
        - application/x-www-form-urlencoded
      parameters:
        - name: account
          description: 使用者帳號
          type: string
          in: formData
          required: true
        - name: password
          description: 使用者密碼
          type: string
          in: formData
          required: true
      responses:
        "200":
          description: Success        

  /user:         

    put:
      description: 更新個人資料
      # movie info to be stored
      consumes:
        - application/x-www-form-urlencoded
      parameters:
        - name: account
          description: 使用者帳號
          type: string
          in: formData
          required: true
        - name: password
          description: 使用者密碼
          type: string
          in: formData
          required: true
        - name: info
          description: 個人資料
          type: string
          in: formData
          required: true
      responses:
        "200":
          description: Success                      
    delete:
      description: 刪除使用者
      # movie info to be stored
      consumes:
        - application/x-www-form-urlencoded
      parameters:
        - name: account
          description: 使用者帳號
          type: string
          in: formData
          required: true
        - name: password
          description: 使用者密碼
          type: string
          in: formData
          required: true
      responses:
        "200":
          description: Success
```

> response code 如果沒寫就看不到response

## Request 參數

```
formData     POST Data
path         parameters, such as /users/{id}
query        parameters, such as /users?role=admin
header       parameters, such as X-MyHeader: Value
cookie       parameters, which are passed in the Cookie header, such as Cookie: debug=0; csrftoken=BUSe35dohU3O1MZvDCU
```

## Schema 型別

```javascript
string (this includes dates and files)
number
integer
boolean
array
object
```

[https://swagger.io/docs/specification/data-models/data-types/#array](https://swagger.io/docs/specification/data-models/data-types/#array)

## 切分區域

用tags，即可將其加入子項目

```
 * /amss1-login/:
 *   get:
 *     summary: Get ProjectRevisions
 *     tags:
 *       - Grid
```
