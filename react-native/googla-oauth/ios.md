# iOS

教學

{% embed url="https://github.com/react-native-google-signin/google-signin/blob/master/docs/ios-guide.md" %}

1.加上 firebase 程式，然後跟著步驟

![](<../../.gitbook/assets/截圖 2020-11-09 下午5.49.50.png>)

2\.  podFile 加上

```
pod 'Firebase/Analytics'
pod 'GoogleSignIn', '~> 5.0.2'
```

然後pod install

3.修改 xcode

![](<../../.gitbook/assets/截圖 2020-11-09 下午6.06.33.png>)

![](<../../.gitbook/assets/截圖 2020-11-09 下午6.06.38.png>)

4.修改 AppDelegate.m

```
// AppDelegate.m
- (BOOL)application:(UIApplication *)application openURL:(nonnull NSURL *)url options:(nonnull NSDictionary<NSString *,id> *)options {
  return [[FBSDKApplicationDelegate sharedInstance] application:application openURL:url options:options] || [RNGoogleSignin application:application openURL:url options:options];
}
```

### 範例：

```javascript
GoogleSignin.configure({
  iosClientId:
    '1795445603utvtntu93ui.apps.googleusercontent.com',
});

// iosClientId 把 GoogleService-Info 這段貼上
// <key>CLIENT_ID</key>
// <string>.....</string>
```
