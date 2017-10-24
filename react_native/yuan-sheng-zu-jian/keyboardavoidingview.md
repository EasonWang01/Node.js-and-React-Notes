以時輸入框要輸入時  彈出的鍵盤會擋到輸入框   所以我們要讓畫面自動上移

```js
  import { KeyboardAvoidingView } from 'react-native';
  
  <KeyboardAvoidingView behavior="padding" style={styles.container}>
                <View style={{flexDirection: 'row', marginTop: 60, alignSelf: 'center'}}>
                  <Text>已經有帳號了嗎?</Text>  
                  <Text style={{fontSize: 15, color: 'blue'}} onPress={() => this.toLogin()}>登入</Text>
                </View>      
  </KeyboardAvoidingView>
```





