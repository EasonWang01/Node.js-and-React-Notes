# facebook oauth

1.先到此創建  
[https://developers.facebook.com/apps/](https://developers.facebook.com/apps/)

2.點選完後點選choose your platform  
即可依照指示完成步驟

加入sdk

```
window.fbAsyncInit = function() {
    FB.init({
      appId      : '259990304339055',
      xfbml      : true,
      version    : 'v2.5'
    });
  };

  (function(d, s, id){
     var js, fjs = d.getElementsByTagName(s)[0];
     if (d.getElementById(id)) {return;}
     js = d.createElement(s); js.id = id;
     js.src = "//connect.facebook.net/en_US/sdk.js";
     fjs.parentNode.insertBefore(js, fjs);
   }(document, 'script', 'facebook-jssdk'));
```

在facebook上填寫[http://localhost:3000](http://localhost:3000)

之後插入這段即可

```
<div
  class="fb-like"
  data-share="true"
  data-width="450"
  data-show-faces="true">
</div>
```

現在有中文版doc直接看即可

[https://developers.facebook.com/docs/facebook-login/web](https://developers.facebook.com/docs/facebook-login/web)



# \#取得個人資料



在這個網址測試，然後跟根據封包內容來傳

https://developers.facebook.com/tools/explorer?method=GET&path=me%3Ffields%3Did%2Cname%2Cpicture%2Cemail%2Cfriendlists&version=v2.8

或是直接在網址輸入

```
https://graph.facebook.com/v2.8/me?access_token=EAADsdbW8NG8BADYzO7XPZCtGpDZC5cuWOZCHBnNhscoZA0hLZBhbaZBgIcB4mN5ZBt4FtivyOENqK6H8eylhk5ywZCraxYQn6QrZAmi4w6Dy8OtVUKSAvZBjii4y91JH2B0s3kTI2xPcXWOlO3t027UnZBWRWnUSHRYWTzKZBlrz7E1BAgZDZD
&fields=id,name,picture,email,friendlists
```



