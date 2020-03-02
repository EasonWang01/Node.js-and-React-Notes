# Alert

可直接使用`alert('訊息')`

另外也可以custom alert

```javascript
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

![](/assets/Screen%20Shot%202018-10-04%20at%202.39.51%20PM.png)

