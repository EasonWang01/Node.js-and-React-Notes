# 以下為Docker執行Redis與Node.js server並分別expose兩個PORT的範例



1.新增package.json

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.13.3"
  }
}
```

2.server.js

```js
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello world\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

3.Dockerfile

```
FROM node:boron

WORKDIR /usr/src/app

RUN apt-get update && apt-get install -y redis-server

COPY package.json .

RUN npm install

# Bundle app source
COPY . .

EXPOSE 8080 6379
CMD [ "npm", "start" ]
```



