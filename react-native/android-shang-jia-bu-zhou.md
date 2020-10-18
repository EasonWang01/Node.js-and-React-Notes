# Android 上架步驟

## 1.更改package name

> 上架時會要求package name必須和上次相同

1.用 Android Studio 打開專案的 android 目錄

![](../.gitbook/assets/jie-tu-20201018-shang-wu-11.26.05.png)

2.在資料夾上點右鍵，然後按 Refactor

![](../.gitbook/assets/jie-tu-20201018-shang-wu-11.26.54.png)

之後上方會跳出藍色的長條 Bar 記得點 Sync now

3.也要更改 build.gradle \(module:app\) 的 applicationId

![](../.gitbook/assets/jie-tu-20201018-shang-wu-11.28.30.png)



## 2.更換ICON

用 Android Asset Studio 工具產生不同大小的圖片後 替換 mipmap 的四個目錄即可。

![](../.gitbook/assets/jie-tu-20201018-shang-wu-11.34.37.png)

[https://medium.com/@scottianstewart/react-native-add-app-icons-and-launch-screens-onto-ios-and-android-apps-3bfbc20b7d4c](https://medium.com/@scottianstewart/react-native-add-app-icons-and-launch-screens-onto-ios-and-android-apps-3bfbc20b7d4c)

## 3.更改APP Name

```text
android/app/src/main/res/values/strings.xml
```

![](../.gitbook/assets/jie-tu-20201018-shang-wu-11.33.25.png)

