---
description: 當 APP 載入時會有空白頁面等待，所以需要一個畫面
---

# Splash screen

{% embed url="https://github.com/crazycodeboy/react-native-splash-screen" %}

1.照著上面的網址步驟設定

2.新增/res/drawable 目錄，裡面放入 launch\_screen.png

3.新增一個 component 如下，並且設置此為 initial Route

```javascript
import React from 'react';
import {View} from 'react-native';
import SplashScreen from 'react-native-splash-screen';
import {getItem} from '../../util/storage.js';

export default class Splash extends React.Component {
  static navigationOptions = {
    header: null,
  };
  componentDidMount() {
    SplashScreen.hide();
    getItem('access_token', (token) => {
      if (!token) {
        this.props.navigation.navigate('Login');
      } else {
        this.props.navigation.navigate('Main');
      }
    });
  }
  render() {
    return <View></View>;
  }
}

```

