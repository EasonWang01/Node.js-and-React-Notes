使用[https://github.com/aksonov/react-native-router-flux/blob/master/docs/MINI\_TUTORIAL.md](https://github.com/aksonov/react-native-router-flux/blob/master/docs/MINI_TUTORIAL.md)

在index新增如下

```
import { AppRegistry } from 'react-native';
import TabViewExample from './src/nav';
import LandingPage from './src/landingPage'


import React, { Component } from 'react';
import { Router, Scene } from 'react-native-router-flux';

export default class App extends Component {
  render() {
    return (
      <Router>
        <Scene key="root">
          <Scene key="pageOne" component={LandingPage} hideNavBar={true} initial={true} />
          <Scene key="main" component={TabViewExample} hideNavBar={true}  />
        </Scene>
      </Router>
    )
  }
}



AppRegistry.registerComponent('AwesomeProject', () =>  App);
```

之後

PageOne

```
import React, { Component } from 'react';
import { View, Text } from 'react-native';
import { Actions } from 'react-native-router-flux';

export default class LandingPage extends Component {
  render() {
    return (
      <View style={{margin: 128}}>
        <Text onPress={Actions.main()}>This is PageOne!</Text>
      </View>
    )
  }
}
```

1.使用action.main來進入於route寫的key的名字的元件

2.使用`hideNavBar={true}`來隱藏上方 `initial={true}`來決定一進入要顯示哪個component

3.帶入文字入A到B畫面可用`Actions.pageTwo({text: 'Hello World!'});`

