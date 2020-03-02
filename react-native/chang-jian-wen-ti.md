# 常見問題

## 環境相關

**0.**

> SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID\_HOME environment variable

解法：在react-native專案的andorid目錄下創建如下local.properties，並寫上路徑。

可直接輸入以下指令

```text
echo sdk.dir = "$HOME"/Library/Android/sdk >> local.properties
```

或是加入環境變量

```text
export ANDROID_HOME = <SDK位置>
```

#### 1.

> Failed to find Build Tools revision

解法：開啟Android Studio後按到plugin manager，之後點選Android SDK Build-tools，然後按右下角的show package Details.

之後更改react-native 的 android folder內的build.gradle ，更改buildToolsVersion

![](/assets/螢幕快照%202018-09-03%20上午9.52.22.png)

#### 2.

> Failed to find Platform SDK with path: platforms;

到SDK頁面把版本號碼，填入剛才build.gradle的SDK版本位置即可。![](/assets/螢幕快照%202018-09-03%20上午9.51.54.png)

**3.**

> Unable to unzip '/Users/lin/.gradle/caches/modules-2/files-2.1/android.arch.lifecycle/runtime/1.0.0/30c60a8a357ee1321ffd0c9f08ef54b24045cd10/runtime-1.0.0.aar' to '/Users/lin/.android/build-cache/461463660edaf90dfdbbbdf1b3fd11a320fb50df/output' or find the cached output '/Users/lin/.android/build-cache/461463660edaf90dfdbbbdf1b3fd11a320fb50df/output' using the build cache at '/Users/lin/.android/build-cache'.

解法：加上sudo

```text
sudo react-native run-android
```

**4.**

> error: could not install \*smartsocket\* listener: Address already in use
>
> 或是
>
> adb server version \(40\) doesn't match this client \(39\);

這時通常輸入`adb start-server` 也會無法執行，通常是因為有兩個版本，所以只要把`/sdk/platform-tools/adb` 複製到`/usr/local/bin`即可

解法：[https://github.com/facebook/react-native/issues/8401\#issuecomment-344628512](https://github.com/facebook/react-native/issues/8401#issuecomment-344628512)

**5.Development Server response 500**

> 通常為node\_module包含不相容的專案

重新啟動專案或重建都一樣時可輸入以下指令：

```text
npm start -- --reset-cache
```

**6.**

不穩定，有時跳出錯誤訊息，有時又沒有

```text
建議關閉 hot-reload 功能。
```

**7.**

安裝模組後如果執行出現錯誤，例如：`Unable to symbolicate stack trace: The stack is null`

輸入以下即可。

```text
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

[https://stackoverflow.com/questions/44446523/unable-to-load-script-from-assets-index-android-bundle-on-windows](https://stackoverflow.com/questions/44446523/unable-to-load-script-from-assets-index-android-bundle-on-windows)

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

設定Adapter 2 的網路為 NAT 即可。 ![](/assets/Screen%20Shot%202019-01-03%20at%204.07.16%20PM.png)

