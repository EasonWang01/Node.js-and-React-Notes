# 使用express

## 撰寫 middleware

在 route 中間加上 

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

