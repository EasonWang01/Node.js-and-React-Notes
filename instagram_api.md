# Instagram API

1.先到如下網站，註冊帳號

https://www.instagram.com/developer

* 
到Authentication可看獲取Access token的方法

* 到EndPoints可看使用API的方法


2.其分為兩種存取API的方式
```
使用三階段(建議此種作法)
client-ID-->code-->access-token

使用兩階段(如沒有server，安全性較低)
client-ID-->access-token


參考如下
https://www.instagram.com/developer/authentication/
```

##範例:
```
https://www.instagram.com/oauth/authorize?client_id=3b6d608eb019431ca90bde60c9178e21&redirect_uri=http://localhost:3000/users/auth/instagram/callback&response_type=token&scope=basic+public_content+follower_list+comments+relationships+likes
```
之後會跳轉到你填寫的redirect URL 並在網址最後面附上`Access-token`
