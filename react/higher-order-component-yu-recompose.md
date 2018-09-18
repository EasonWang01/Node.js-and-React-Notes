# Higher order component

> A higher-order component \(HOC\) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature.

[https://reactjs.org/docs/higher-order-components.html](https://reactjs.org/docs/higher-order-components.html)

可想像是我們有一個共用的邏輯要在許多component中共用，我們可以把邏輯寫在一個component中，然後使用例如：

```js
<WrappedComponent
  injectedProp={injectedProp}
  {...passThroughProps}
/>
```

如此將component包在HOC中，共用邏輯。

> ```
> You can imagine that in a large app, this same pattern of subscribing to DataSource and calling setState will occur over and over again. We want an abstraction that allows us to define this logic in a single place and share it across many components. This is where higher-order components excel.
> ```

# Recompose

[https://github.com/acdlite/recompose](https://github.com/acdlite/recompose)

> Recompose is a React utility belt for function components and higher-order components. Think of it like lodash for React.
>
> 在Function component 與 higher-order components 不能使用例如setState等方法，所以可以用recompose之API來加上。
>
> 例如：加上一個counter state，然後用setCounter方法改變state
>
> ```js
> const enhance = withState('counter', 'setCounter', 0)
> const Counter = enhance(({ counter, setCounter }) =>
>   <div>
>     Count: {counter}
>     <button onClick={() => setCounter(n => n + 1)}>Increment</button>
>     <button onClick={() => setCounter(n => n - 1)}>Decrement</button>
>   </div>
> )
> ```

最後組成component時可寫為如下：

```js
const BookingFormWithProps = compose(
  setDisplayName('BookingForm'),
  withRouter,
  connect(mapStateToProps),
  withReload,
  withStateHandlers(localState(), localStateHandlers),
  Form.create({
    onValuesChange: executeSideEffects,
  }),
  withPreloadAndListener,
  mapProps(filterToBookingFormProps),
)(BookingForm);
```

管理state可用withStateHandlers

EX:

```js
const BookingFormWithProps = compose(....
withStateHandlers(localState(), localStateHandlers);
....)


localState寫上state的值
localStateHandler寫上改變state的function
```



