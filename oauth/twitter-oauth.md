# Twitter OAuth

目前有 1.0a 與 2.0 版本，1.0a 能使用的功能較多，以下討論 1.0a。

主要流程為:

1\. twitter developer portal 註冊 app 後拿到 Consumer Key 與 Consumer Secret

2.用戶點選 UI 同意我們的 APP 授權。

3.獲取，之後拿到 oauth\_token 與 oauth\_verifier

4.發送 API 到 twitter 獲取 access\_token

[https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)
