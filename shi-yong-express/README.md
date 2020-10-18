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

