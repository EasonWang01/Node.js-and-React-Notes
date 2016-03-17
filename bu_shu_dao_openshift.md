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

5.在poenshift上建立新的application並填入你的public ssh 的內容

6.新建一個資料夾，先git init後git clone加上poenshift產生給你的ssh url，(注意:如果詢問yes/no時，要輸入yes，不可直接按enter)


```
你剛新建的資料夾--->nodejs資料夾
```



使用putty登入openshift:
https://www.youtube.com/watch?v=dZwngyEtWmU
必須先用git等工具產生ssh key 
如果登入錯誤，可重新產生putty的ppk file即可，記得用private的


參考至:https://blog.openshift.com/run-your-nodejs-projects-on-openshift-in-two-simple-steps/

