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

