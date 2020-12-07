---
description: 當 APP 載入時會有空白頁面等待，所以需要一個畫面
---

# Splash screen

{% embed url="https://github.com/crazycodeboy/react-native-splash-screen" %}

1.照著上面的網址步驟設定

2.新增/res/drawable 目錄，裡面放入 launch\_screen.png

3.App.js 搭配 react-navigation 配置如下

```javascript
import React from 'react';
import Login from './pages/Login/';
import Main from './pages/Main/';
import 'react-native-gesture-handler';
import {NavigationContainer} from '@react-navigation/native';
import {createStackNavigator} from '@react-navigation/stack';
import SplashScreen from 'react-native-splash-screen';
import {getItem} from './util/storage';
const Stack = createStackNavigator();

export default class App extends React.Component {
  state = {
    hasToken: false,
    loadFinish: false,
  };
  componentDidMount() {
    getItem('access_token', (token) => {
      if (!token) {
        this.setState({hasToken: false}, () => {
          this.setState({loadFinish: true});
          SplashScreen.hide();
        });
      } else {
        this.setState({hasToken: true}, () => {
          this.setState({loadFinish: true});
          SplashScreen.hide();
        });
      }
    });
  }
  render() {
    return this.state.loadFinish ? (
      <NavigationContainer>
        <Stack.Navigator
          initialRouteName={this.state.hasToken ? 'Main' : 'Login'}
          screenOptions={{
            headerShown: false,
          }}>
          <Stack.Screen name="Login" component={Login} />
          <Stack.Screen name="Main" component={Main} />
        </Stack.Navigator>
      </NavigationContainer>
    ) : (
      <></>
    );
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

