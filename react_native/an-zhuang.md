# 使用Expo

> npm建議版本 4.6.1  or 5.3.0  \(其他容易會有安裝package失敗問題\)
>
> 記得執行的時候genymotion 執行的Android版本要跟你安裝的Android SDK有對應。

可以不用安裝Xcode或android直接開發，再跑起來之後可以用實機掃描QRCODE即會顯示在自己的手機上，或是下載genymotion即可顯示在模擬器，或`create-react-native-app run ios` 他會自動打開IOS模擬器

> 使用expo 在 eject前無法放入native的相關模組
>
> 參考:[https://github.com/react-community/create-react-native-app/blob/master/EJECTING.md](https://github.com/react-community/create-react-native-app/blob/master/EJECTING.md)

但是Expo也有提供許多SDK\(包含oauth\)

```
https://docs.expo.io/versions/v16.0.0/sdk/facebook.html#expofacebookloginwithreadpermissionsasyncappid-options
```

安裝手冊[https://github.com/react-community/create-react-native-app/blob/master/react-native-scripts/template/README.md](https://github.com/react-community/create-react-native-app/blob/master/react-native-scripts/template/README.md)

論壇:[https://forums.expo.io/](https://forums.expo.io/)

#### Android on OSX

```
1.下載virtualBox 與 Genymotion
2.npm install create-react-native-app -g
3.create-react-native-app my-app 
4.cd my-app; create-react-native-app run android
```

> 可能會要你同意permission \(Draw over other apps 這時打勾後點上一頁即可

#### Android on Windows

步驟和上面相同  之後  下載Android 的expo app然後掃描qr code

```
如果想用模擬器的方法
必須要把genymotion 的setting 將 adb路徑加入
https://github.com/react-community/create-react-native-app/blob/master/react-native-scripts/template/README.md#using-genymotions-adb

可參考https://stackoverflow.com/questions/35959350/react-native-android-genymotion-adb-server-didnt-ack

所以必須安裝android SDK
```

> 之後開發時存檔建議按兩下  時常會發生按一下不會自動reload問題

# 使用Expo

可以使用`yarn eject` 來還原為原本專案。

