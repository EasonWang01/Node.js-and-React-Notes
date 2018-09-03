# 環境

#### 0.

SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID\_HOME environment variable

解法：在react-native專案的andorid目錄下創建如下local.properties，並寫上路徑。

可直接輸入以下指令

```
echo sdk.dir = "$HOME"/Library/Android/sdk >> local.properties
```

### 1.

問題：Failed to find Build Tools revision

解法：開啟Android Studio後按到plugin manager，之後點選Android SDK Build-tools，然後按右下角的show package Details.

之後更改react-native 的 android folder內的build.gradle ，更改buildToolsVersion

![](/assets/螢幕快照 2018-09-03 上午9.52.22.png)

### 2.

Failed to find Platform SDK with path: platforms;

到SDK頁面把版本號碼，填入剛才build.gradle的SDK版本位置即可。![](/assets/螢幕快照 2018-09-03 上午9.51.54.png)

#### 3.

Unable to unzip '/Users/lin/.gradle/caches/modules-2/files-2.1/android.arch.lifecycle/runtime/1.0.0/30c60a8a357ee1321ffd0c9f08ef54b24045cd10/runtime-1.0.0.aar' to '/Users/lin/.android/build-cache/461463660edaf90dfdbbbdf1b3fd11a320fb50df/output' or find the cached output '/Users/lin/.android/build-cache/461463660edaf90dfdbbbdf1b3fd11a320fb50df/output' using the build cache at '/Users/lin/.android/build-cache'.

解法：加上sudo

```
sudo react-native run-android
```

# 開發

# 1.無法用e.target

建議採用map並加上index方法傳遞。

[https://stackoverflow.com/a/42125039](https://stackoverflow.com/a/42125039)

```js
            {Object.keys(this.state.locationURL).map((location, idx) => (
              <ListItem key={idx} onPress={(e) => this.changeURL(idx)}>
                <Text>{location}</Text>
              </ListItem>
            ))}
```



