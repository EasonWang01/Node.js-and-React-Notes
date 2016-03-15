# EJS

類似於Handlebars的模板引擎

安裝好Express後用

```

app.set('view engine', 'ejs');
```
引入即可，之後創建views資料夾，資料夾內檔案後檔名取.ejs

##!!如出現找不到ejs module
1.原因為如果是使用-g的express必須將ejs也安裝在-g
2.或是將express和ejs都安裝在loacl

