# 自動化測試

使用 Dexon

[https://github.com/wix/Detox/tree/master/docs](https://github.com/wix/Detox/tree/master/docs)

搭配 Jest

{% embed url="https://github.com/wix/Detox/blob/master/docs/Guide.Jest.md" %}

```text
yarn add jest jest-circus detox --dev
npx detox init -r jest
```

之後會出現四個檔案

1. detoxrc.json
2. e2e/config.json
3. e2e/enviroment.js
4. firstTest.e2e.js

於 Android 目錄下輸入 `./gradlew assembleDebug`

然後去修改 .detoxrc.json 中的 binarypath 與 avdName 類似如下

```text
    "android": {
      "type": "android.emulator",
      "binaryPath": "/Users/easonwang/MySampleApp/android/app/build/outputs/apk/debug/app-debug.apk",
      "device": {
        "avdName": "Pixel_XL_API_30"
      }
    }
```

> 但目前 React Native 0.63 還是無法執行，會有 timeout 情況  
> [https://stackoverflow.com/questions/61050417/detoxruntimeerror-failed-to-run-application-on-the-device-android](https://stackoverflow.com/questions/61050417/detoxruntimeerror-failed-to-run-application-on-the-device-android)

```text
npx detox test -c android
```

## Android 記得去設定 emulator

[https://github.com/wix/Detox/blob/master/docs/Introduction.AndroidDevEnv.md](https://github.com/wix/Detox/blob/master/docs/Introduction.AndroidDevEnv.md)

