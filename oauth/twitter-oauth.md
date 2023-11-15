# Twitter OAuth

目前有 1.0a 與 2.0 版本，1.0a 能使用的功能較多，

2.0 主要是用來獲取公開 API 資訊，不用用戶授權。

## Twitter Oauth 1.0a

主要流程為:

&#x20; 1\. twitter developer portal 註冊 app 後拿到 Consumer Key 與 Consumer Secret

2. 用 Consumer Key 與 Consumer Secret 發送請求到： [https://api.twitter.com/oauth/request\_token](https://api.twitter.com/oauth/request\_token) ，之後拿到 oauth\_token 與 oauth\_token\_secret
3. 用 oauth\_token 與 oauth\_token\_secret 給用戶在頁面 UI 授權，之後拿到 callback\_url 會拿到 oauth\_token 與 oauth\_verifier，需要比對 oauth\_token 與第二步驟的相同。
4. 用 oauth\_consumer\_key 與 oauth\_token, oauth\_verifier 發送 API 到 twitter 獲取 access\_token

[https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)
