# 註解寫在Code內生成swagger UI

## 安裝

```
yarn add swagger-jsdoc swagger-ui-express swagger-model-validator
```

## 使用

1. 新增： /router/swagger.js

```js
const express = require('express')
const router = express.Router()

const options = {
  swaggerDefinition: {
    info: {
      title: 'REST - Swagger',
      version: '1.0.0',
      description: 'REST API with Swagger doc',
      contact: {
        email: ''
      }
    },
    tags: [
      {
        name: 'AMSS5',
        description: 'AMSS5 API'
      }
    ],
    schemes: ['http'],
    host: 'localhost:3124',
    basePath: '/api'
  }, // 下面記得要寫上要讀哪邊的註解
  apis: ['./router/grid.js', './router/project.js', './router/user.js', './database/schemas/grid.js']
}

const swaggerJSDoc = require('swagger-jsdoc')
const swaggerUi = require('swagger-ui-express')
const swaggerSpec = swaggerJSDoc(options)
require('swagger-model-validator')(swaggerSpec)

router.get('/json', function (req, res) {
  res.setHeader('Content-Type', 'application/json')
  res.send(swaggerSpec)
})

router.use('/', swaggerUi.serve, swaggerUi.setup(swaggerSpec))

function validateModel (name, model) {
  const responseValidation = swaggerSpec.validateModel(name, model, false, true)
  if (!responseValidation.valid) {
    console.error(responseValidation.errors)
    throw new Error(`Model doesn't match Swagger contract`)
  }
}

module.exports = router;
```

2.綁定swagger UI router

server.js

```js
const swaggerRoute = require("./router/swagger");
app.use("/swagger", swaggerRoute);
```

1. API 文件

```js
/**
 * @swagger
 * /:
 *   get:
 *     description: Create Grid
 *     tags:
 *       - Grid
 *     produces:
 *       - application/json
 *     responses:
 *       200:
 *         description: stocks
 *         schema:
 *           $ref: '#/definitions/Stocks'
 */

 或是

 /**
 * @swagger
 * /:
 *   post:
 *     summary: Add a new pet
 *     parameters:
 *       - in: formData
 *         name: userId
 */


router.post('/', async (req, res) => {
 .......
})
```

1. Model 文件

```js
/**
 * @swagger
 * definitions:
 *   Stock:
 *     type: object
 *     required:
 *       - id
 *       - name
 *       - currentPrice
 *       - lastUpdate
 *     properties:
 *       id:
 *         type: number
 *       name:
 *         type: string
 *       currentPrice:
 *         type: number
 *       lastUpdate:
 *         type: number
 *   Stocks:
 *     type: array
 *     items:
 *       $ref '#definitions/Stock'
 */
const GridSchema = new Schema({ 
 ......
})
```

5.瀏覽 swagger UI

[http://localhost:3000/swagger/api\#/](http://localhost:3124/swagger/api#/)

> port 為你 server 的 port

參考至：[https://danielpecos.com/2017/09/06/rest-api-with-node-js-and-swagger/](https://danielpecos.com/2017/09/06/rest-api-with-node-js-and-swagger/)

6.瀏覽產生出的swagger.json

http://localhost:3000/[swagger/json](https://amss-5-portfolio.bridge5.asia/swagger/json)

# 加入Redoc

Redoc提供更完整的文件頁面

[https://github.com/Redocly/redoc](https://github.com/Redocly/redoc)

#### 使用方式

在html加入如下

```html
<!DOCTYPE html>
<html>
  <head>
    <title>ReDoc</title>
    <!-- needed for adaptive design -->
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://fonts.googleapis.com/css?family=Montserrat:300,400,700|Roboto:300,400,700" rel="stylesheet">

    <!--
    ReDoc doesn't change outer page styles
    -->
    <style>
      body {
        margin: 0;
        padding: 0;
      }
    </style>
  </head>
  <body>
    <redoc spec-url='http://petstore.swagger.io/v2/swagger.json'></redoc>
    <script src="https://cdn.jsdelivr.net/npm/redoc@next/bundles/redoc.standalone.js"> </script>
  </body>
</html>
```

把上面的swagger.json替換為你的即可

