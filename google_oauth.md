# google oauth
1.先到dev console申請帳號和註冊應用程式
https://console.developers.google.com/project/_/apiui/apis/library

2.到api管理員-->憑證-->已授權的 JavaScript 來源
，填入http://localhost:3000
(記得，下面的已授權的重新導向 URI不要填)

3.加入這段到網頁
```
<script src="https://apis.google.com/js/platform.js" async defer></script>
```
4.加入meta tag到網頁(後面要改為你自己的client id)
```
<meta name="google-signin-client_id" content="YOUR_CLIENT_ID.apps.googleusercontent.com">
```
5.直接放入google做好的signin button
```
<div class="g-signin2" data-onsuccess="onSignIn"></div>
```