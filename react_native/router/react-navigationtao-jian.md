# react-navigation套件

[https://reactnavigation.org/docs/en/navigating.html](https://reactnavigation.org/docs/en/getting-started.html)

以下將建立一個登入畫面以及登入後進入主畫面，下方顯示三個可導航列表

#### Login.js

```js
import React from 'react';
import {
  Image,
  Platform,
  ScrollView,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
} from 'react-native';

export default class Login extends React.Component {
  static navigationOptions = {
    header: null,
  };

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.test} onPress={() => this.props.navigation.navigate('Main')}>登入</Text>
      </View>
    );
  }
}
const styles = StyleSheet.create({
  test: {
    marginTop: 200,
    textAlign: 'center'
  }
});
```

> 使用`this.props.navigation.navigate('Main')` 進行導航

#### MainTabNavigator.js

主畫面下方的導航列表

```js
import React from 'react';
import { Platform } from 'react-native';
import { createStackNavigator, createBottomTabNavigator } from 'react-navigation';

import TabBarIcon from '../components/TabBarIcon';
import HomeScreen from '../screens/HomeScreen';
import LinksScreen from '../screens/LinksScreen';
import SettingsScreen from '../screens/SettingsScreen';


const HomeStack = createStackNavigator({
  Home: HomeScreen,
});

HomeStack.navigationOptions = {
  tabBarLabel: 'Home',
  tabBarIcon: ({ focused }) => (
    <TabBarIcon
      focused={focused}
      name={
        Platform.OS === 'ios'
          ? `ios-information-circle${focused ? '' : '-outline'}`
          : 'md-information-circle'
      }
    />
  ),
};

const LinksStack = createStackNavigator({
  Links: LinksScreen,
});

LinksStack.navigationOptions = {
  tabBarLabel: 'Links',
  tabBarIcon: ({ focused }) => (
    <TabBarIcon
      focused={focused}
      name={Platform.OS === 'ios' ? `ios-link${focused ? '' : '-outline'}` : 'md-link'}
    />
  ),
};

const SettingsStack = createStackNavigator({
  Settings: SettingsScreen,
});

SettingsStack.navigationOptions = {
  tabBarLabel: 'Settings',
  tabBarIcon: ({ focused }) => (
    <TabBarIcon
      focused={focused}
      name={Platform.OS === 'ios' ? `ios-options${focused ? '' : '-outline'}` : 'md-options'}
    />
  ),
};

export default createBottomTabNavigator({
  HomeStack,
  LinksStack,
  SettingsStack,
});
```

> 使用createStackNavigator來初始化該路由，然後用createBottomTabNavigator建立底部導航

#### AppNavigator.js

將登入與主畫面整合

```js
import React from 'react';
import { createSwitchNavigator, createStackNavigator } from 'react-navigation';
import LoginScreen from '../screens/Login.js'
import MainTabNavigator from './MainTabNavigator';
const AuthStack = createStackNavigator({ SignIn: LoginScreen });

export default createSwitchNavigator({
  // You could add another route here for authentication.
  // Read more at https://reactnavigation.org/docs/en/auth-flow.html
  Auth: AuthStack,
  Main: MainTabNavigator,
},
  {
    initialRouteName: 'Auth',
  }
);
```



