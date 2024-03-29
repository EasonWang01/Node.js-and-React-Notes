# Android 上架步驟

## 1.更改package name

> 上架時會要求package name必須和上次相同

1.用 Android Studio 打開專案的 android 目錄

![](<../.gitbook/assets/截圖 2020-10-18 上午11.26.05.png>)

2.在資料夾上點右鍵，然後按 Refactor

![](<../.gitbook/assets/截圖 2020-10-18 上午11.26.54.png>)

之後上方會跳出藍色的長條 Bar 記得點 Sync now

3.也要更改 build.gradle (module:app) 的 applicationId

![](<../.gitbook/assets/截圖 2022-05-06 上午11.31.06.png>)

4.更改 Androidmanifest package name

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.test">
```

還有 setting.gradle 與 app.json 都要改

## 2.更換 Android APP Icon

1. **前往 Android Asset Studio 網站**：首先，打開瀏覽器，前往 Android Asset Studio 網站[https://romannurik.github.io/AndroidAssetStudio](https://romannurik.github.io/AndroidAssetStudio/%EF%BC%89%E3%80%82)
2. **選擇圖示類型**：在 Android Asset Studio 中，您可以選擇不同類型的圖示，例如應用程式圖示、圖示標籤等。選擇您要生成的圖示類型。
3. **設置圖示參數**：根據您的需求，設置圖示的參數，如圖示顏色、背景形狀、大小等。可以根據不同分辨率需求生成不同大小的圖示。
4. **生成圖示**：完成設置後，點擊生成圖示。Android Asset Studio 將生成所需大小和風格的圖示。
5. **下載生成的圖示**：生成完成，下載壓縮文件，其中包含了各種不同分辨率的圖示。
6. **解壓縮生成的圖示**：將下載的壓縮文件解壓縮到一個適當的位置，以便後續使用。
7. **替換 mipmap 目錄**：現在，打開 Android 項目，導航到 res 目錄，有四個不同的 mipmap 目錄，通常分別是 mipmap-mdpi、mipmap-hdpi、mipmap-xhdpi 和 mipmap-xxhdpi。將生成的圖示文件複製到每個相應的目錄中。

## 3.更改APP Name

```
android/app/src/main/res/values/strings.xml
```

![](<../.gitbook/assets/截圖 2022-05-06 上午11.33.44.png>)

## 4. 打包成 APK

1.輸入以下產生金鑰，除了密碼外其他可以不用輸入直接按 enter

```
keytool -genkeypair -v -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

2.之後把剛才產生的金鑰放在 android/app 目錄下

3.修改 android/gradle.properties ，新增以下，並把 \*\*\* 改為你剛輸入的密碼

```
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=*****
MYAPP_UPLOAD_KEY_PASSWORD=*****
```

4.修改 android/app/build.gradle

```
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
                storeFile file(MYAPP_UPLOAD_STORE_FILE)
                storePassword MYAPP_UPLOAD_STORE_PASSWORD
                keyAlias MYAPP_UPLOAD_KEY_ALIAS
                keyPassword MYAPP_UPLOAD_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
```

5\. 打包 成 aab file

```
cd android
./gradlew bundleRelease
```

> 因為現在上架要求包含 32 與 64 位元版本，所以建議使用 bundleRelease 打包成 AAB (Android App Bundle)，但因為 aab 在手機無法測試，可以打包成 apk 先測試

如果想打包成 apk 可用如下

```
./gradlew assembleRelease
```

> 如果 network endpoint 指向 http 記得加上以下不然會無法連線\
> [https://stackoverflow.com/a/56801525/4622645](https://stackoverflow.com/a/56801525/4622645)

打包完會看到如下檔案

![](<../.gitbook/assets/截圖 2020-10-18 上午11.46.44.png>)

{% embed url="https://reactnative.dev/docs/signed-apk-android" %}

6.去 Android developer console

[https://play.google.com/console/](https://play.google.com/console/)

登入之後並點選建立新版本。

> 如果登入後一直跳出另一個帳戶，記得把 cookie 清空後重新登入。

![](<../.gitbook/assets/截圖 2020-10-18 上午11.52.24.png>)

7.如果之前有發佈過，記得更改版本號

![](<../.gitbook/assets/截圖 2022-05-06 上午11.35.36.png>)
