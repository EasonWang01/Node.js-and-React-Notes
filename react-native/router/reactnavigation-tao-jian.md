# react-navigation套件

[https://reactnavigation.org/docs/en/navigating.html](https://reactnavigation.org/docs/en/getting-started.html)

#### 1.隱藏Header，於各router 的 component

> ```javascript
> class .....
>   static navigationOptions = {
>     header: null,
>   };
>   .....
> }
> ```
>
> [https://stackoverflow.com/questions/44701245/hide-header-in-stack-navigator-react-navigation](https://stackoverflow.com/questions/44701245/hide-header-in-stack-navigator-react-navigation)

v5:&#x20;

```javascript
 <Stack.Navigator
     screenOptions={{
       headerShown: false,
     }}
 ...
```

#### 2.進行導航

使用`this.props.navigation.navigate('Main')`&#x20;

> 使用createStackNavigator來初始化該路由，然後用createBottomTabNavigator建立底部導航

v5:

```javascript
const Login = ({ navigation }) => {
  navigation.navigate('Main');
}
```

## 範例：

[https://snack.expo.io/@react-navigation/going-back-v2](https://snack.expo.io/@react-navigation/going-back-v2)

```javascript
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

## 監聽Router Change

> react-navigation listen on router change

1\.

```javascript
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

> ```
> onWillFocus={payload => console.log("will focus", payload)}
> onDidFocus={payload => console.log("did focus", payload)}
>
> 這兩個有時要同時加才有反應
> ```
