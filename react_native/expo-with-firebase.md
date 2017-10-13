# \#Expo with Firebase

Expo可以直接使用js的SDK在React Native內和firebase溝通

[https://docs.expo.io/versions/latest/guides/using-firebase.html](https://docs.expo.io/versions/latest/guides/using-firebase.html)

[https://firebase.google.com/docs/database/web/read-and-write](https://firebase.google.com/docs/database/web/read-and-write)

Ex:

```js
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import * as firebase from 'firebase';
var firebaseConfig = {
  apiKey: "AIzaSyDtHB9OP0RWG04RtD3aOs5yeVauDLrNUaa",
  authDomain: "test-214e1.firebaseapp.com",
  databaseURL: "https://test-314e1.firebaseio.com",
  storageBucket: "test-214e1.appspot.com"
};
firebase.initializeApp(firebaseConfig);
export default class App extends React.Component {
  componentDidMount() {
    console.log(1234)
    function storeHighScore(userId, score) {
      firebase.database().ref('users/' + userId).set({
        highscore: score
      });
    }
    storeHighScore(2,100)

    return firebase.database().ref('/users/1').once('value').then(function(snapshot) {
      console.log(snapshot.val())
      // ...
    });
  }
  render() {
    return (
      <View style={styles.container}>
        <Text>{firebase.app().name}</Text>
        <Text>Changes you make will automatically reload.</Text>
        <Text>12ˇˇ2hake your phone to open the developer menu.</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

即可在此看到https://console.firebase.google.com/project/test-214e1/database/data/

