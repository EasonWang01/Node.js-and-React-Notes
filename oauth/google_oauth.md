# google oauth

## 2022 06更新

```markup
<html>
  <body>
    <script src="https://accounts.google.com/gsi/client" async defer></script>
    <div id="g_id_onload"
         data-client_id=""
         data-callback="handleCredentialResponse">
    </div>
    <div class="g_id_signin" data-type="standard"></div>
  </body>
  <script>
    function handleCredentialResponse(e){
      console.log(e)
    }
  </script>
</html>
```

> 如果出現網域不再許可範圍內記得加入以下，並且加入沒有 port 的網域
>
> [https://console.cloud.google.com/apis/credentials/oauthclient/](https://console.cloud.google.com/apis/credentials/oauthclient/)

![](<../.gitbook/assets/截圖 2022-06-01 下午1.58.03.png>)



1.先到dev console申請帳號和註冊應用程式 [https://console.developers.google.com/project/\_/apiui/apis/library](https://console.developers.google.com/project/\_/apiui/apis/library)

2.到api管理員-->憑證-->已授權的 JavaScript 來源 ，填入[http://localhost:3000](http://localhost:3000) (記得，下面的已授權的重新導向 URI不要填)

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

6.此時已可正常登入，但我們看不到任何登入後的訊息，所以要加入下面這個function

```
function onSignIn(googleUser) {
  var profile = googleUser.getBasicProfile();
  console.log('ID: ' + profile.getId()); // Do not send to your backend! Use an ID token instead.
  console.log('Name: ' + profile.getName());
  console.log('Image URL: ' + profile.getImageUrl());
  console.log('Email: ' + profile.getEmail());
}
```

7.如何登出

```
<a href="#" onclick="signOut();">Sign out</a>
<script>
  function signOut() {
    var auth2 = gapi.auth2.getAuthInstance();
    auth2.signOut().then(function () {
      console.log('User signed out.');
    });
  }
</script>
```

8.傳送使用者id到server做認證 (如同官方告知的profile.getId()不安全，所以要用另一個方式)

```
 var id_token = googleUser.getAuthResponse().id_token;
```

將剛才那段function改為

```
  <script>
function onSignIn(googleUser) {
  var profile = googleUser.getBasicProfile();
  console.log('ID: ' + profile.getId()); // Do not send to your backend! Use an ID token instead.
  console.log('Name: ' + profile.getName());
  console.log('Image URL: ' + profile.getImageUrl());
  console.log('Email: ' + profile.getEmail());
   var id_token = googleUser.getAuthResponse().id_token;
   console.log(id_token);
}
  </script>
```

之後會在console看到如下

```
eyJhbGciOiJSUzI1NiIsImtpZCI6ImQyMjBhOGQzYmRmYTYxNjRkMzIwZTFlMDkxMGRiMDM4MGQ0NWMwNzMifQ.eyJpc3MiOiJhY2NvdW50cy5nb29nbGUuY29tIiwiYXRfaGFzaCI6ImRic2dwRW5WWWJCQUFjSzI2QnBmRlEiLCJhdWQiOiI5ODMwMzAzODI1MTAtNDFjYWN0MjA3ZjJvNTZxb202b2x2N3RjbWlpNmRlYzAuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMDk0NDgxMDM1OTU3NDQ2Mzc5MjgiLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYXpwIjoiOTgzMDMwMzgyNTEwLTQxY2FjdDIwN2YybzU2cW9tNm9sdjd0Y21paTZkZWMwLmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwiZW1haWwiOiJqYXNvbjQwMTE1QHlhaG9vLmNvbS50dyIsImlhdCI6MTQ1ODUzOTgxMiwiZXhwIjoxNDU4NTQzNDEyLCJuYW1lIjoi546LIEVhc29uIiwicGljdHVyZSI6Imh0dHBzOi8vbGg1Lmdvb2dsZXVzZXJjb250ZW50LmNvbS8teE50ZUN1WnUxOFkvQUFBQUFBQUFBQUkvQUFBQUFBQUFCMzQvR01WTF9aRkl4d1Uvczk2LWMvcGhvdG8uanBnIiwiZ2l2ZW5fbmFtZSI6IueOiyIsImZhbWlseV9uYW1lIjoiRWFzb24
```

9.從client傳送這個給server即可

```
var xhr = new XMLHttpRequest();
xhr.open('POST', 'https://yourbackend.example.com/tokensignin');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onload = function() {
  console.log('Signed in as: ' + xhr.responseText);
};
xhr.send('idtoken=' + id_token);
```

## 進階認證

10.但，使用者可能得到你的post位置後，直接傳入此位置一個他人的(id\_token)，為了避免這樣的情況，我們可以在server端再次傳送id\_token給google提供的endpoint做認證

```
https://www.googleapis.com/oauth2/v3/tokeninfo?id_token=XYZ123
```

使用get方式傳給上述地址，而把id\_token改為你要傳的id\_token即可

12.把我們剛才從網頁console下來的id\_token直接加在上面的網址後然後貼到瀏覽器進入該網址

如token正確會得到如下

```
{
 "iss": "accounts.google.com",
 "at_hash": "dbsgnVYbBAAcK26BpfFQ",
 .....
 }
```

如錯誤會得到如下

```
{
 "error_description": "Invalid Value"
}
```

最後再檢查上面的aud是否與你自己在google dev console註冊到的用戶端 ID相同即可

範例:

```
app.post("/signin",function(req,res){
console.log(req.body);

request('https://www.googleapis.com/oauth2/v3/tokeninfo?id_token='+req.body.idtoken, function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the Google homepage. 
    var isLogin = true;
          }
        });

});
```

### 另外google提供的Google API Client Library

(有興趣可參考)

[https://developers.google.com/api-client-library/javascript/dev/dev\_jscript#setting-credentials](https://developers.google.com/api-client-library/javascript/dev/dev\_jscript#setting-credentials)

參考至: [https://developers.google.com/identity/sign-in/web/sign-in#before\_you\_begin](https://developers.google.com/identity/sign-in/web/sign-in#before\_you\_begin)
