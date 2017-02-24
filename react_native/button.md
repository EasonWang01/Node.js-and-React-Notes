原生的按鈕沒有background，且color行為在android和ios不同

https://facebook.github.io/react-native/docs/button.html

所以我們可用其他第三方組件

https://github.com/ide/react-native-button


ex:

```
  <Button
    containerStyle={{padding:10, height:45, overflow:'hidden', borderRadius:4, backgroundColor: 'yellow'}}
    style={{fontSize: 20, color: 'green'}}>
    Press me!
  </Button>

```

