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

## Android 設置

```text
yarn add react-native-splash-screen
npx react-native link react-native-splash-screen
```

然後

Update the `MainActivity.java` to use `react-native-splash-screen` via the following changes:

```text
import android.os.Bundle; // here
import com.facebook.react.ReactActivity;
// react-native-splash-screen >= 0.3.1
import org.devio.rn.splashscreen.SplashScreen; // here
// react-native-splash-screen < 0.3.1
import com.cboy.rn.splashscreen.SplashScreen; // here

public class MainActivity extends ReactActivity {
   @Override
    protected void onCreate(Bundle savedInstanceState) {
        SplashScreen.show(this);  // here
        super.onCreate(savedInstanceState);
    }
    // ...other code
}
```

然後 Create a file called `launch_screen.xml` in `app/src/main/res/layout` \(create the layout-folder if it doesn't exist\). The contents of the file should be the following:

```text
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ImageView android:layout_width="match_parent" android:layout_height="match_parent" android:src="@drawable/launch_screen" android:scaleType="centerCrop" />
</RelativeLayout>
```

以及 Add a color called `primary_dark` in `app/src/main/res/values/colors.xml`

```text
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="primary_dark">#000000</color>
</resources>
```

