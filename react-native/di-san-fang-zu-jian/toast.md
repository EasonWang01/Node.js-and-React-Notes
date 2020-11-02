# Toast

{% embed url="https://github.com/crazycodeboy/react-native-easy-toast" %}



```javascript
import Toast, {DURATION} from 'react-native-easy-toast';

        <Toast
          ref="toast"
          style={{backgroundColor: 'black'}}
          position="bottom"
          positionValue={200}
          fadeInDuration={750}
          fadeOutDuration={1000}
          opacity={0.8}
          textStyle={{color: 'white'}}
        />
```

```javascript
this.refs.toast.show('回覆成功', DURATION.LENGTH_LONG);
```



