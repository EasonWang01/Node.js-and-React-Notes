# 常見問題

## Android 環境相關

**0.**

> SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID\_HOME environment variable

解法：在react-native專案的andorid目錄下創建如下local.properties，並寫上路徑。

可直接輸入以下指令

```
echo sdk.dir = "$HOME"/Library/Android/sdk >> ./android/local.properties
```

或是加入環境變量

```
export ANDROID_HOME = <SDK位置>
```

#### 1.

> Failed to find Build Tools revision

解法：開啟Android Studio後按到plugin manager，之後點選Android SDK Build-tools，然後按右下角的show package Details.

之後更改react-native 的 android folder內的build.gradle ，更改buildToolsVersion

#### 2.

> Failed to find Platform SDK with path: platforms;

到SDK頁面把版本號碼，填入剛才build.gradle的SDK版本位置即可。

**3.**

> Unable to unzip '/Users/lin/.gradle/caches/modules-2/files-2.1/android.arch.lifecycle/runtime/1.0.0/30c60a8a357ee1321ffd0c9f08ef54b24045cd10/runtime-1.0.0.aar' to '/Users/lin/.android/build-cache/461463660edaf90dfdbbbdf1b3fd11a320fb50df/output' or find the cached output '/Users/lin/.android/build-cache/461463660edaf90dfdbbbdf1b3fd11a320fb50df/output' using the build cache at '/Users/lin/.android/build-cache'.

解法：加上sudo

```
sudo react-native run-android
```

**4.**

> error: could not install \*smartsocket\* listener: Address already in use
>
> 或是
>
> adb server version (40) doesn't match this client (39);

這時通常輸入`adb start-server` 也會無法執行，通常是因為有兩個版本，所以只要把`/sdk/platform-tools/adb` 複製到`/usr/local/bin`即可

解法：[https://github.com/facebook/react-native/issues/8401#issuecomment-344628512](https://github.com/facebook/react-native/issues/8401#issuecomment-344628512)

**5.Development Server response 500**

> 通常為node\_module包含不相容的專案

重新啟動專案或重建都一樣時可輸入以下指令：

```
npm start -- --reset-cache
```

**6.**

不穩定，有時跳出錯誤訊息，有時又沒有

```
建議關閉 hot-reload 功能。
```

**7.**

安裝模組後如果執行出現錯誤，例如：`Unable to symbolicate stack trace: The stack is null`

輸入以下即可。

```
npm start -- reset-cache
```

**8.**

執行 `react-native run-android` 後畫面空白，或是說無法連線到development server

> 重新執行`react-native run-android`
>
> 即可

**9.**

避免特定資料夾更改後，觸發 react native server hot reload

[https://stackoverflow.com/a/41963217](https://stackoverflow.com/a/41963217)

```javascript
const blacklist = require('metro-config/src/defaults/blacklist');

// blacklist is a function that takes an array of regexes and combines
// them with the default blacklist to return a single regex.
// 下面輸入不要觸發更新的資料夾名稱
module.exports = {
  resolver: {
    blacklistRE: blacklist([/server\/.*/])
  }
};
```

**10.Unable to load script from assets index.android.bundle**

加入以下：

package.json

```javascript
  "scripts": {
    "android-linux": "react-native bundle --platform android --dev false --entry-file App.js --bundle-output android/app/src/main/assets/App.android.bundle --assets-dest android/app/src/main/res && react-native run-android"
  },
```

{% embed url="https://stackoverflow.com/questions/44446523/unable-to-load-script-from-assets-index-android-bundle-on-windows" %}

### 11.Failed to install the following Android SDK packages as some licences have not been accepted

到 android studio 安裝 SDK 版本

![](<../.gitbook/assets/螢幕快照 2020-10-15 下午4.06.49.png>)

#### 12.Could not determine artifacts for com.facebook.fresco:fresco:1.10.0: Skipped due to earlier error

在 build.gradle 新增

```markup
dependencies {
    ....
    implementation "com.facebook.fresco:fresco:1.13.0"
}
```

13\. 上架時要求更改 package name

{% embed url="http://hklifenote.blogspot.com/2015/06/android-studiopackage.html" %}

14\. 您已經有一個 APK 或 Android App Bundle 使用版本代碼 1

{% embed url="http://snowsfox.blogspot.com/2017/05/apk_38.html" %}

15\. Google Play 64 位元規範

build.gradle(app) 新增如下

```
    defaultConfig {
        ....
        ndk {
            abiFilters 'armeabi-v7a','arm64-v8a','x86','x86_64'
        }
    }
```

> 不過目前測試完加上這樣會閃退，建議還是使用 bundleRelease 打包

{% embed url="https://developer.android.com/distribute/best-practices/develop/64-bit" %}

16\. adb command not found

加上以下環境變數，不過目前測試就算 adb comand 沒找到還是可以 run-android，但有時候使用最新的 react-native 創建專案卻可以沒有adb command 下執行

```
export ANDROID_HOME="/Users/easonwang/Library/Android/sdk"
export PATH="/Users/easonwang/Library/Android/sdk/platform-tools":$PATH
```

17\. react version mismatch

通常只要把所有監聽 8081 (emulator)的 port 刪除，或是關閉所有 terminal 重啟 emulator即可。

18.產生的apk 安裝後網路無法連線

> 如果 network endpoint 指向 http 記得加上以下不然會無法連線\
> [https://stackoverflow.com/a/56801525/4622645](https://stackoverflow.com/a/56801525/4622645)

19.Warning: Activity not started, intent has been delivered to currently running top-most instance.

這樣 app 不護更新畫面，需要把 app 從模擬器關閉後重新 run-android

20\. Execution failed for task ':app:mergeDexDebug'

> When your app and the libraries it references exceed 65,536 methods, you encounter a build error that indicates your app has reached the limit of the Android build architecture

加上以下即可

```
android {
    defaultConfig {
        ...
        minSdkVersion 15 
        targetSdkVersion 28
        multiDexEnabled true
    }
    ...
}

dependencies {
  implementation 'com.android.support:multidex:1.0.3'
}
```

{% embed url="https://developer.android.com/studio/build/multidex" %}

21\.

> Execution failed for task ':app:processDebugResources'.  AAPT: error: unexpected element \<queries> found in \<manifest>

更新 gradle 版本：

project/build.gradle

```javascript
    dependencies {
        classpath("com.android.tools.build:gradle:4.0.1")
```

{% embed url="https://stackoverflow.com/a/62969918/4622645" %}

22.No non-density apk found

```
cd android && ./gradlew clean
```

23.Error type 3 Error: Activity class {} does not exist.

```
adb uninstall PACKAGE_NAME
```

24.accessibilityinfo could not be found within the project

```
watchman watch-del-all;
rm rf ./node_modules;
yarn;
yarn start --reset-cache;
rm -rf /tmp/metro-*;
```

25.重新安裝 debug.keystore file

[https://stackoverflow.com/questions/57016236/keystore-file-project-folder-android-app-debug-keystore-not-found-for-signing](https://stackoverflow.com/questions/57016236/keystore-file-project-folder-android-app-debug-keystore-not-found-for-signing)

## 開發相關

## 1.無法用e.target

建議採用map並加上index方法傳遞。

[https://stackoverflow.com/a/42125039](https://stackoverflow.com/a/42125039)

```javascript
{Object.keys(this.state.locationURL).map((location, idx) => (
   <ListItem key={idx} onPress={(e) => this.changeURL(idx)}>
     <Text>{location}</Text>
   </ListItem>
))}
```

**2.取得Input的值或對象**

```javascript
<Input onChangeText={(e) => console.log(e)} />
```

**3. Genymotion 存取跟電腦相同的 VPN**

設定Adapter 2 的網路為 NAT 即可。&#x20;

4.重置 ios 或 android 專案

```
rm -rf ./ios
yarn add react-native-eject
npx react-native eject
```

## iOS 環境相關

#### Could not find the following native modules

```
rm -rf ios/Pods && rm -rf ios/build && cd ios && pod install && cd ../

rm -rf node_modules && rm yarn.lock && yarn install
```

#### Pod version 不夠

```
sudo gem install cocoapods
```

#### 如果出現    CompileC /Users/Library/Developer/Xcode/DerivedData/rnVideoChat-glonxrdqrszkjpegwxeamdfiunvm/Build/Intermediates.noindex/Pods.build/Debug-iphonesimulator/react-native-webrtc.build/Objects-normal/x86\_64/VideoCaptureController.o /Users/easonwang/rnVideoChat/node\_modules/react-native-webrtc/ios/RCTWebRTC/VideoCaptureController.m normal x86\_64 objective-c com.apple.compilers.llvm.clang.1\_0.compiler

> 重新 npm 安裝該模組，升級他的版本即可

### Build fail code 65

這時可以去 xcode 查看相關錯誤，或是先更新 xcode。

例如：[https://stackoverflow.com/questions/50718018/xcode-10-error-multiple-commands-produce](https://stackoverflow.com/questions/50718018/xcode-10-error-multiple-commands-produce)
