# Docker Compose

寫好許多DockerFIle 後可以用 Docker Compose 快速啟動多個 container，原本要在docker run 寫的參數也可以寫在 docker-compose.yml 內。



## 新增 volume 讓外部更改可以反映到內部

> 用docker 跑 server 後可以在電腦上改code 影響到內部

1. DockerFile

```text
FROM node:10

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8081
CMD [ "node", "server.js" ]
```

2. .dockerignore

```text
node_modules
npm-debug.log
```

3.package.json

```text
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
    "express": "^4.16.1"
  }
}
```

4. server.js

```javascript
const express = require('express');

// Constants
const PORT = 8081;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
    res.sendFile(`${__dirname}/src/index.html`);
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

5. ./src/index.html

```markup
<!DOCTYPE html>
<html>
<input id="fileInput" type="file"></input>
<button onclick="uploadFile()">上傳</button>
<script>
    function uploadFile() {
        const fileInput = document.querySelector('#fileInput');
        const file = fileInput.files[0];
        const reader = new FileReader();
        reader.readAsArrayBuffer(file);
        reader.onload = function () {
            const arrayBuffer = reader.result
            const bytes = new Uint8Array(arrayBuffer);
            console.log(bytes)
            fetch('http://localhost:5000', {
                method: 'POST', // or 'PUT'
                body: JSON.stringify({
                  file: bytes
                }), // data can be `string` or {object}!
                headers: new Headers({
                    'Content-Type': 'application/json'
                })
            }).then(res => res.json())
                .catch(error => console.error('Error:', error))
                .then(response => console.log('Success:', response));
        }
    }
</script>

</html>
```

6. docker-compose.yml

> volumes 把現在資料夾的 src mapping 到 docker 內的 /usr/src/app/src ，讓我們更改外部時同時會改 docker 內部的檔案

```text
version: '3' # 目前使用的版本，可以參考官網：
services: # services 關鍵字後面列出 web, redis 兩項專案中的服務
  web:
    build: . # Build 在同一資料夾的 Dockerfile（描述 Image 要組成的 yaml 檔案）成 container
    ports:
      - "49160:8081" # 外部露出開放的 port 對應到 docker container 的 port
    volumes: 
      - "./src:/usr/src/app/src"  
```

之後執行

```text
docker-compose up
```

然後 到 [http://localhost:49160/](http://localhost:49160/) 看到網頁後試著更改 ./src/index.html 即可看到改變。

