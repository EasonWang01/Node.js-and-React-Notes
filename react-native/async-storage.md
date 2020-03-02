# Async Storage



## AsyncStorage

用來儲存內容，類似localstorage。

[https://facebook.github.io/react-native/docs/asyncstorage](https://facebook.github.io/react-native/docs/asyncstorage)

```javascript
import { AsyncStorage } from 'react-native';

_storeData = async () => {
  try {
    await AsyncStorage.setItem('@MySuperStore:key', 'I like to save it.');
  } catch (error) {
    // Error saving data
  }
};

_retrieveData = async () => {
  try {
    const value = await AsyncStorage.getItem('TASKS');
    if (value !== null) {
      // We have data!!
      console.log(value);
    }
  } catch (error) {
    // Error retrieving data
  }
};
```

