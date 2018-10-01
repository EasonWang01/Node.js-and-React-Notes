Drawer.js

```js
import React from 'react';
import { DrawerLayoutAndroid, View, Text } from 'react-native';
export default class Drawer extends React.Component {
  componentDidMount() {
    this.props.onRef(this)
  }
  openTheDrawer(){
    this.drawer.openDrawer();
  };

  render() {
    var navigationView = (
      <View style={{ flex: 1, backgroundColor: '#fff' }}>
        <Text style={{ margin: 10, fontSize: 15, textAlign: 'left' }}>I'm in the Drawer!</Text>
      </View>
    );
    return (
      <DrawerLayoutAndroid
        ref={(_drawer) => this.drawer = _drawer}
        drawerWidth={300}
        drawerPosition={DrawerLayoutAndroid.positions.Left}
        renderNavigationView={() => navigationView}>
        {this.props.children}
      </DrawerLayoutAndroid>
    )
  }
}
```

使用

```js
  openDrawer = () => {
    this.drawer.openTheDrawer();
  };
  render() {
    return (
      <Drawer onRef={ref => (this.drawer = ref)}>
       .......
      </Drawer>
      )
  }
```



