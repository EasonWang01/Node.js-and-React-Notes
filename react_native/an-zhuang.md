# \#使用Expo

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

```
步驟和上面相同  只是要把genymotion的adb路徑加入
```



