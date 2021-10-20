# Auth Code Input

[https://github.com/retyui/react-native-confirmation-code-field](https://github.com/retyui/react-native-confirmation-code-field)

```javascript
import {
  CodeField,
  Cursor,
  useBlurOnFulfill,
  useClearByFocusCell,
} from 'react-native-confirmation-code-field';

const CELL_COUNT = 6;


const [authCodeValue, setAuthCodeValue] = useState('');
const refAuthCode = useBlurOnFulfill({authCodeValue, cellCount: CELL_COUNT});
const [props, getCellOnLayoutHandler] = useClearByFocusCell({
  authCodeValue,
  setAuthCodeValue,
});

<SafeAreaView style={styles.root}>
  <Text style={styles.title}>Verification</Text>
  <CodeField
    ref={refAuthCode}
    {...props}
    value={authCodeValue}
    onChangeText={setAuthCodeValue}
    cellCount={CELL_COUNT}
    rootStyle={styles.codeFieldRoot}
    keyboardType="number-pad"
    textContentType="oneTimeCode"
    renderCell={({index, symbol, isFocused}) => (
      <View
        // Make sure that you pass onLayout={getCellOnLayoutHandler(index)} prop to root component of "Cell"
        onLayout={getCellOnLayoutHandler(index)}
        key={index}
        style={[styles.cellRoot, isFocused && styles.focusCell]}>
        <Text style={styles.cellText}>
          {symbol || (isFocused ? <Cursor /> : null)}
        </Text>
      </View>
    )}
  />
</SafeAreaView>


const styles = {
  // Auth Code Input
  root: {padding: 20, minHeight: 300},
  title: {textAlign: 'center', fontSize: 30},
  codeFieldRoot: {
    marginTop: 20,
    width: 280,
    marginLeft: 'auto',
    marginRight: 'auto',
  },
  cellRoot: {
    width: '10%',
    height: 60,
    justifyContent: 'center',
    alignItems: 'center',
    borderBottomColor: '#ccc',
    borderBottomWidth: 1,
  },
  cellText: {
    color: '#000',
    fontSize: 36,
    textAlign: 'center',
  },
  focusCell: {
    borderBottomColor: '#007AFF',
    borderBottomWidth: 2,
  },
  //
};
```

![](<../../.gitbook/assets/截圖 2021-03-31 下午6.12.05.png>)
