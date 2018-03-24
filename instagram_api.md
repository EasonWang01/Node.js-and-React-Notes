# Instagram API

改版後期改用sandbox模式，意思為，所有app要先經過審核才可使用public\_content部分，一開始你只可讀取到你自己的content，或是你可以把別人邀請入你的sandbox，之後其同意後你也可以API獲取其資訊，不然都需經過審核才可使用API

1.先到如下網站，註冊帳號

[https://www.instagram.com/developer](https://www.instagram.com/developer)

* 到Authentication可看獲取Access token的方法

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

## 範例:

```
https://www.instagram.com/oauth/authorize?client_id=3b6d608eb019431ca90bde60c9178e21&redirect_uri=http://localhost:3000/users/auth/instagram/callback&response_type=token&scope=basic+public_content+follower_list+comments+relationships+likes
```

之後會跳轉到你填寫的redirect URL 並在網址最後面附上`Access-token`





## 查詢指定tag的所有文章

GET

```
https://www.instagram.com/explore/tags/輸入欲查詢的TAG/?__a=1
```

## 查詢使用者文章

GET

```
https://www.instagram.com/使用者ID/?__a=1
```

# 注意事項：

2017/10/1號之後只能取得Basic的資訊，其他 API 都不開放了。

[https://www.instagram.com/developer/changelog/](https://www.instagram.com/developer/changelog/)

