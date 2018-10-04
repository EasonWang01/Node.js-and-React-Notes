可直接使用`alert('訊息')`

另外也可以custom alert

```js
 import { Alert } from 'react-native';

Alert.alert(
  'Alert Title',
  'My Alert Msg',
  [
    {text: 'Ask me later', onPress: () => console.log('Ask me later pressed')},
    {text: 'Cancel', onPress: () => console.log('Cancel Pressed'), style: 'cancel'},
    {text: 'OK', onPress: () => console.log('OK Pressed')},
  ],
  { cancelable: false }
)
```

![](/assets/Screen Shot 2018-10-04 at 2.39.51 PM.png)

