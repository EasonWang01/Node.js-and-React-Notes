# 部屬到OpenShift

## 部屬到OpenShift

1.[https://www.openshift.com/](https://www.openshift.com/) 先註冊帳號

2.將入口js檔案名稱改為server.js

3.使用npm init，並確認script的start為server.js

```
"scripts": {
  "start": "node server.js"
},
"main": "server.js"
```

4.使用git工具產生ssh key，之後會有兩個檔案其中有.pub後檔名的為public key [https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

5.在openshift上建立新的application並

```
首頁=>My account=>web console=>Add application
=>選最右下角的Node.js
```

之後點選setting填入你的public ssh 的內容

```
最上方點setting=>於右方框，複製你電腦產生的.pub內容後貼上
```

6.新建一個資料夾，先git init後git clone openshift產生的url，(注意:如果詢問yes/no時，要輸入yes，不可直接按enter)

7.刪掉clone下來資料夾內的全部內容，貼上你自己的

8.確認package.json中的dependencies都有寫上，node\_module都已安裝在local資料夾(不可是全域)

9.記得將原本server.js中的app.listen改為

```
app.set('port', process.env.OPENSHIFT_NODEJS_PORT || process.env.PORT || 3002);
app.set('ip', process.env.OPENSHIFT_NODEJS_IP || "127.0.0.1");


app.listen(app.get('port') ,app.get('ip'), function () {
    console.log("✔ Express server listening at %s:%d ", app.get('ip'),app.get('port'));

});
```

10.cd到你的application名稱的資料夾內使用git bash上傳

(記得，要進入application名稱資料夾，而不是原本clone用的資料夾)

```
git add .
git commit -m "save"
git push
```

PS:因為openshift會自動看你的package.json幫你安裝module 所以不用把node\_modules加入

## 如果上傳出現無法連到8080port，則使用ssh連到openshift進行Debug

1\. 使用putty登入openshift: [https://www.youtube.com/watch?v=dZwngyEtWmU](https://www.youtube.com/watch?v=dZwngyEtWmU)

必須先用git等工具產生ssh key 如果登入錯誤，可重新從puttygen產生putty的ppk file即可，記得用private的

host name的格式如下

```
56ea4f4889f5cfa5c700010a@forclass-yicheng01.rhcloud.com
```

2.登入後檔案存放在

```
app-root--->runtime---->repo   以及
app-root---->repo
```

如果要看log

```
app-root/logs/nodejs.log
```

參考至:[https://blog.openshift.com/run-your-nodejs-projects-on-openshift-in-two-simple-steps/](https://blog.openshift.com/run-your-nodejs-projects-on-openshift-in-two-simple-steps/)

(PS:上面官網範例的step2的code block中的listen括號寫錯位置，需自行更改)

## 如果有用到websocket

openShift預設讓websocket監聽8000port

以socket.io為例

```
 var socket = io.connect("http://chat-yicheng01.rhcloud.com:8000");
```
