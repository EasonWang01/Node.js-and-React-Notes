# 使用express

## 撰寫 middleware

在 route 中間加上&#x20;

```javascript
function (req, res, next) {
  ....
  next()// 如果沒執行 next 則不會往下執行
}
```

```javascript
app.get('/user/:id', function (req, res, next) {
  console.log('ID:', req.params.id);
  next();
}, function (req, res, next) {
  res.send('User Info');
});
```

## 增加 Request logger

```javascript
const logger = require('morgan');
const fs = require('fs');

logger.token('body', (req) => {
  return JSON.stringify(req.body)
})
app.use(logger(':remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] :body', {stream: fs.createWriteStream('./access.log', {flags: 'a'})}))
```

## 快速讀取 JSON 後回傳 API

```javascript
fs.createReadStream(filePath).pipe(res);
```

如果需要修改 JSON 可使用以下

{% embed url="https://github.com/dominictarr/JSONStream" %}
\`
{% endembed %}

```javascript
const JSONStream = require("JSONStream");

const stream = fs.createReadStream(filePath, { encoding: "utf8" }),
parser = JSONStream.parse();

stream.pipe(parser);

parser.on("data", function (obj) {
  console.log(obj); // whatever you will do with each JSON object
});
```
