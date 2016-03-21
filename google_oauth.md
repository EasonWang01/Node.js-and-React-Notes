# google OAuth
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
  8.傳送使用者id到server做認證
(如同官方告知的profile.getId()不安全，所以要用另一個方式)
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
傳送這個給database即可
