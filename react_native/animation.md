# React Native Animation

有四種 component可以加上動畫 `View`,`Text`,`image` , `scrollView`

如果要擴展到其他 component 可以使用 `Animated.createAnimatedComponent()`

## 使用

```js
import { Animated, LayoutAnimation  } from 'react-native';
```

> [`LayoutAnimation`](https://facebook.github.io/react-native/docs/animations#layoutanimation-api) 可以讓你一次控制所有畫面的 animation
>
> [`Animated`](https://facebook.github.io/react-native/docs/animations#animated-api) 用來控制單一元件動畫。

#### 範例：

```js
class FadeInView extends React.Component {
  state = {
    fadeAnim: new Animated.Value(0),  // Initial value for opacity: 0
  }

  componentDidMount() {
    Animated.timing(                  // Animate over time
      this.state.fadeAnim,            // The animated value to drive
      {
        toValue: 1,                   // Animate to opacity: 1 (opaque)
        duration: 10000,              // Make it take a while
      }
    ).start();                        // Starts the animation
  }

  render() {
    let { fadeAnim } = this.state;

    return (
      <Animated.View                 // Special animatable View
        style={{
          ...this.props.style,
          opacity: fadeAnim,         // Bind opacity to animated value
        }}
      >
        {this.props.children}
      </Animated.View>
    );
  }
}
```

之後即可使用

```js
<FadeInView style={{width: 250, height: 50, backgroundColor: 'powderblue'}}>
  <Text style={{fontSize: 28, textAlign: 'center', margin: 10}}>Fading in</Text>
</FadeInView>
```

# 相關模組

[https://github.com/oblador/react-native-animatable](https://github.com/oblador/react-native-animatable)



