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

4.

建立image

\(-t 為tag 的意思即為這個image的名字\)

```
docker build -t test/node-web-app .
```

5.

執行image

\(意思為把Node.js server之8080port開放給外面電腦的49160，與開放Redis的6379\)

```
 docker run -p 49160:8080 49161:6379 test/node-web-app
```



