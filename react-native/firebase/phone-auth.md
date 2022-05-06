---
description: 每月可以免費寄送一萬個認證簡訊。
---

# Phone Auth

{% embed url="https://rnfirebase.io/auth/phone-auth" %}

{% embed url="https://v5.rnfirebase.io/docs/v5.x.x/auth/phone-auth" %}

1.記得先去 Firebase console 開啟 phone auth 服務。

![](<../../.gitbook/assets/截圖 2021-03-26 下午5.45.18.png>)

> RN 跟 web 不同，不需要 initializeApp()

## iOS

2.加入 [URL scheme](https://stackoverflow.com/questions/61514076/firebase-phone-auth-getting-ios-error-register-custom-url-scheme) 到 `info.plist`

> open the GoogleService-Info.plist configuration file, and look for the REVERSED\_CLIENT\_ID key and add to info.plist

例如:

```
	<key>CFBundleURLTypes</key>
	<array>
		<dict>
			<key>CFBundleTypeRole</key>
			<string>Editor</string>
			<key>CFBundleURLName</key>
			<string></string>
			<key>CFBundleURLSchemes</key>
			<array>
				<string>com.googleusercontent.apps....-...</string>
			</array>
		</dict>
	</array>
```

[https://stackoverflow.com/questions/61514076/firebase-phone-auth-getting-ios-error-register-custom-url-scheme](https://stackoverflow.com/questions/61514076/firebase-phone-auth-getting-ios-error-register-custom-url-scheme)

3.可以到下方改變簡訊內容 (非必要)

![](<../../.gitbook/assets/截圖 2021-03-26 下午5.46.59.png>)

### 範例：

```javascript
import React, {useState} from 'react';
import auth from '@react-native-firebase/auth';
import {Text, View, TextInput, Button} from 'react-native';

function PhoneSignIn() {
  // If null, no SMS has been sent
  const [confirm, setConfirm] = useState(false);

  const [code, setCode] = useState('');

  // Handle the button press
  async function signInWithPhoneNumber(phoneNumber) {
    const confirmation = await auth().signInWithPhoneNumber(phoneNumber);
    setConfirm(confirmation);
  }

  async function confirmCode() {
    try {
      let result = await confirm.confirm(code);
      console.log(result);
    } catch (error) {
      console.log('Invalid code.');
    }
  }

  if (!confirm) {
    return (
      <Button
        title="Phone Number Sign In"
        onPress={() => signInWithPhoneNumber('+886...')}
      />
    );
  }

  return (
    <>
      <Text>Phone Auth</Text>
      <TextInput
        style={{borderBottomWidth: 1, borderColor: 'black'}}
        value={code}
        onChangeText={text => setCode(text)}
      />
      <Button title="Confirm Code" onPress={() => confirmCode()} />
    </>
  );
}

const App = () => {
  return (
    <View style={{padding: '10%', paddingTop: '50%'}}>
      <Text style={{fontSize: 20, textAlign: 'center'}}>Phone Auth 範例</Text>
      <View>
        <PhoneSignIn />
      </View>
    </View>
  );
};

export default App;

```

之後你的手機會收到簡訊。

![](<../../.gitbook/assets/截圖 2021-03-26 下午5.54.24.png>)

Tip: 另外也可以新增測試用號碼，避免重複發送多次後被暫停使用

![](<../../.gitbook/assets/截圖 2021-03-26 下午6.15.22.png>)

## Android

> Androidn 流程稍微比較多

1.加入相關 Gradle script (app/build.gradle)

```javascript
dependencies {
    implementation 'androidx.browser:browser:1.3.0'
    implementation 'com.google.android.gms:play-services-safetynet:18.0.1'
    implementation platform('com.google.firebase:firebase-bom:29.3.1')
    implementation 'com.google.firebase:firebase-analytics'
```

[https://rnfirebase.io/#configure-firebase-with-android-credentials](https://rnfirebase.io/#configure-firebase-with-android-credentials)

2.加入 sha 指紋：

獲取 sha1, sha256 hash 後加入 firebase console

```
keytool -list -v -alias androiddebugkey -keystore ./android/app/debug.keystore
```

{% embed url="https://rnfirebase.io/#generating-android-credentials" %}

3\. 加入測試用電話

> 這邊加入後使用測試電話發簡訊時則不會實際收到簡訊，但如果沒加入測試電話每天只能發 50 個簡訊

#### 可能錯誤

1\.

```
Error: [auth/app-not-authorized] This app is not authorized to use Firebase Authentication. Please verify that the correct package name and SHA-1 are configured in the Firebase Console. [ A safety_net_token was passed, but no matching SHA-256 was registered in the Firebase console. Please make sure that this application’s packageName/SHA256 pair is registered in the Firebase Console. ]
```

這邊要確認下載 android/app 內的 debug.keystore 裡面的sha1, sha256 是不是就是這把 key的，有時會讀取到根目錄下的 \~/.android/debug.keystore
