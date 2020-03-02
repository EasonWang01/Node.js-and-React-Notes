# Expo

## 使用Expo

> npm建議版本 4.6.1 or 5.3.0 \(其他容易會有安裝package失敗問題\)
>
> 記得執行的時候genymotion 執行的Android版本要跟你安裝的Android SDK有對應。

可以不用安裝Xcode或android直接開發，再跑起來之後可以用實機掃描QRCODE即會顯示在自己的手機上，或是下載genymotion即可顯示在模擬器，或`create-react-native-app run ios` 他會自動打開IOS模擬器

> 使用expo 在 eject前無法放入native的相關模組
>
> 參考:[https://github.com/react-community/create-react-native-app/blob/master/EJECTING.md](https://github.com/react-community/create-react-native-app/blob/master/EJECTING.md)

但是Expo也有提供許多SDK\(包含oauth\)

```text
https://docs.expo.io/versions/v16.0.0/sdk/facebook.html#expofacebookloginwithreadpermissionsasyncappid-options
```

安裝手冊[https://github.com/react-community/create-react-native-app/blob/master/react-native-scripts/template/README.md](https://github.com/react-community/create-react-native-app/blob/master/react-native-scripts/template/README.md)

論壇:[https://forums.expo.io/](https://forums.expo.io/)

**Android on OSX**

```text
1.下載virtualBox 與 Genymotion
2.npm install create-react-native-app -g
3.create-react-native-app my-app 
4.cd my-app; create-react-native-app run android
```

> 可能會要你同意permission \(Draw over other apps 這時打勾後點上一頁即可

**Android on Windows**

步驟和上面相同 之後 下載Android 的expo app然後掃描qr code

```text
如果想用模擬器的方法
必須要把genymotion 的setting 將 adb路徑加入
https://github.com/react-community/create-react-native-app/blob/master/react-native-scripts/template/README.md#using-genymotions-adb

可參考https://stackoverflow.com/questions/35959350/react-native-android-genymotion-adb-server-didnt-ack

所以必須安裝android SDK
```

> 之後開發時存檔建議按兩下 時常會發生按一下不會自動reload問題

## 

可以使用`yarn eject` 來還原為原本專案。

## 

## Expo with Firebase

Expo可以直接使用js的SDK在React Native內和firebase溝通

[https://docs.expo.io/versions/latest/guides/using-firebase.html](https://docs.expo.io/versions/latest/guides/using-firebase.html)

[https://firebase.google.com/docs/database/web/read-and-write](https://firebase.google.com/docs/database/web/read-and-write)

Ex:

```javascript
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import * as firebase from 'firebase';
var firebaseConfig = {
  apiKey: "...",
  authDomain: "test-214e1.firebaseapp.com",
  databaseURL: "https://test-314e1.firebaseio.com",
  storageBucket: "test-214e1.appspot.com"
};
firebase.initializeApp(firebaseConfig);
export default class App extends React.Component {
  componentDidMount() {
    console.log(1234)
    function storeHighScore(userId, score) {
      firebase.database().ref('users/' + userId).set({
        highscore: score
      });
    }
    storeHighScore(2,100)

    return firebase.database().ref('/users/1').once('value').then(function(snapshot) {
      console.log(snapshot.val())
      // ...
    });
  }
  render() {
    return (
      <View style={styles.container}>
        <Text>{firebase.app().name}</Text>
        <Text>Changes you make will automatically reload.</Text>
        <Text>shake your phone to open the developer menu.</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

即可在此看到[https://console.firebase.google.com/project/test-214e1/database/data/](https://console.firebase.google.com/project/test-214e1/database/data/)

