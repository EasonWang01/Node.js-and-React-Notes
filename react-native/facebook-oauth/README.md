# Facebook OAuth



[https://github.com/facebook/react-native-fbsdk](https://github.com/facebook/react-native-fbsdk)

[https://developers.facebook.com/docs/android/getting-started/](https://developers.facebook.com/docs/android/getting-started/)

參考上述兩個網頁設定。

新增權限

```text
<LoginButton
  publishPermissions={["email", "user_friends", "user_photos"]}
....
```

但是要注意現在許多權限都要審核並申請。

![](/assets/螢幕快照%202019-05-31%20下午1.47.31.png)  
[https://developers.facebook.com/apps/350004755369462/app-review/permissions/](https://developers.facebook.com/apps/350004755369462/app-review/permissions/)

## 使用

```text
import { LoginButton, AccessToken, LoginManager } from 'react-native-fbsdk';
```

1.使用內建UI button

```javascript
  <LoginButton
          readPermissions={["email", "public_profile"]}
          onLoginFinished={
            (error, result) => {
              if (error) {
                console.log("login has error: " + result.error);
              } else if (result.isCancelled) {
                console.log("login is cancelled.");
              } else {
                AccessToken.getCurrentAccessToken().then(
                  async (data) => {
                    const FBtoken = data.accessToken.toString();
                    await saveStorage('FBtoken', FBtoken);
                    const userFbInfo = await get(`https://graph.facebook.com/me?access_token=${FBtoken}&fields=id,name,picture,email,friendlists,birthday`);
                    await saveStorage('userFbInfo', JSON.stringify(
                      userFbInfo
                    ));
                    context.props.goMainPage();
                  }
                )
              }
            }
          }
          onLogoutFinished={() => this.logoutFinish()} 
    />
```

2.自定義UI，使用Login Manager

```javascript
<Button
  title="Continute with FB"
  onPress={() => this.FBLoginTrigger()}
/>

FBLoginTrigger() {
    const context = this;
    LoginManager.logInWithReadPermissions(["email", "public_profile"]).then(
      function (result) {
        if (result.isCancelled) {
          console.log('Login was cancelled');
        } else {
          AccessToken.getCurrentAccessToken().then(
            async (data) => {
              const FBtoken = data.accessToken.toString();
              await saveStorage('FBtoken', FBtoken);
              const userFbInfo = await get(`https://graph.facebook.com/me?access_token=${FBtoken}&fields=id,name,picture,email,friendlists,birthday`);
              await saveStorage('userFbInfo', JSON.stringify(
                userFbInfo
              ));
              context.props.goMainPage();
            }
          )
        }
      },
      function (error) {
        console.log('Login failed with error: ' + error);
      }
    );
  }
```



