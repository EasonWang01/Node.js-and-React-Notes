[https://github.com/facebook/react-native-fbsdk](https://github.com/facebook/react-native-fbsdk)

[https://developers.facebook.com/docs/android/getting-started/](https://developers.facebook.com/docs/android/getting-started/)

參考上述兩個網頁設定。

新增權限

```
<LoginButton
  publishPermissions={["email", "user_friends", "user_photos"]}
....  
```

但是要注意現在許多權限都要審核並申請。

![](/assets/螢幕快照 2019-05-31 下午1.47.31.png)  
[https://developers.facebook.com/apps/350004755369462/app-review/permissions/](https://developers.facebook.com/apps/350004755369462/app-review/permissions/)

