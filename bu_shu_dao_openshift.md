# 部屬到OpenShift

1.https://www.openshift.com/ 先註冊帳號

2.將入口js檔案名稱改為server.js

3.使用npm init，並確認script的start為server.js
```
"scripts": {
  "start": "node server.js"
},
"main": "server.js"
```
4.使用git工具產生ssh key，之後會有兩個檔案其中有.pub後檔名的為public key
https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

5.在openshift上建立新的application並填入你的public ssh 的內容

6.新建一個資料夾，先git init後git clone加上poenshift產生給你的ssh url，(注意:如果詢問yes/no時，要輸入yes，不可直接按enter)

7.確認package.json中的dependencies都有寫上，_node_module都以安裝在local資料夾(不可是全域)

8.記得將原本server.js中的app.listen改為
```
app.set('port', process.env.OPENSHIFT_NODEJS_PORT || process.env.PORT || 3002);
app.set('ip', process.env.OPENSHIFT_NODEJS_IP || "127.0.0.1");


app.listen(app.get('port') ,app.get('ip'), function () {
    console.log("✔ Express server listening at %s:%d ", app.get('ip'),app.get('port'));
   
});
```

9.cd到server.js下使用git bash上傳


```
你剛新建的資料夾--->nodejs資料夾
```


#如果上傳出現無法連到8080port，則使用ssh連到openshift進行Debug
-----

使用putty登入openshift:
https://www.youtube.com/watch?v=dZwngyEtWmU

必須先用git等工具產生ssh key 
如果登入錯誤，可重新產生putty的ppk file即可，記得用private的

host name的格式如下
```
56ea4f4889f5cfa5c700010a@forclass-yicheng01.rhcloud.com
```

登入後進app-root--->repo

參考至:https://blog.openshift.com/run-your-nodejs-projects-on-openshift-in-two-simple-steps/

