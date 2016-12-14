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



https://developers.facebook.com/docs/facebook-login/web

