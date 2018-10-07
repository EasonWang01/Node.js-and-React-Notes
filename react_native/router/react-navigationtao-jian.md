# react-navigation套件

[https://reactnavigation.org/docs/en/navigating.html](https://reactnavigation.org/docs/en/getting-started.html)

> 隱藏Header
>
> ```js
> static navigationOptions = {
>   header: null,
> };
> ```
>
> 使用`this.props.navigation.navigate('Main')` 進行導航
>
> 使用createStackNavigator來初始化該路由，然後用createBottomTabNavigator建立底部導航

# 或是直接貼上以下Code即可

[https://snack.expo.io/@react-navigation/going-back-v2](https://snack.expo.io/@react-navigation/going-back-v2)

```js
import React from 'react';
import { Button, View, Text } from 'react-native';
import { StackNavigator } from 'react-navigation'; // Version can be specified in package.json

class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        <Button
          title="Go to Details"
          onPress={() => this.props.navigation.navigate('Details')}
        />
      </View>
    );
  }
}

class DetailsScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Details Screen</Text>
        <Button
          title="Go to Details... again"
          onPress={() => this.props.navigation.push('Details')}
        />
        <Button
          title="Go to Home"
          onPress={() => this.props.navigation.navigate('Home')}
        />
        <Button
          title="Go back"
          onPress={() => this.props.navigation.goBack()}
        />
      </View>
    );
  }
}

const RootStack = StackNavigator(
  {
    Home: {
      screen: HomeScreen,
    },
    Details: {
      screen: DetailsScreen,
    },
  },
  {
    initialRouteName: 'Home',
  }
);

export default class App extends React.Component {
  render() {
    return <RootStack />;
  }
}
```



# 監聽Router Change

> react-navigation listen on router change

1.

```js
import { NavigationEvents } from "react-navigation";

  render() {
    return (
      <View>
        <NavigationEvents
          onWillFocus={payload => console.log("will focus", payload)}
          onDidFocus={payload => console.log("did focus", payload)}
          onWillBlur={payload => console.log("will blur", payload)}
          onDidBlur={payload => console.log("did blur", payload)}
        />
        ..........
```

使用NavigationEvents即可。

