# Compose

[https://github.com/reduxjs/redux/blob/master/src/compose.js](https://github.com/reduxjs/redux/blob/master/src/compose.js)

```js
function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```

使用

```
let a = (c) => c * c
let b = (x) => x + x
compose(a, b)(1)
// 4
```

> 因為 compose\(a, b\)\(1\)  會變為 `a(b(1))`



