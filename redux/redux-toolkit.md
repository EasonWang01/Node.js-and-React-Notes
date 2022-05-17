---
description: 前言：如果還不太了解 Redux 概念可以看上一篇
---

# Redux Toolkit

可以省去部分步驟，簡單地設定 Redux

## 安裝

```
yarn add react-redux redux @reduxjs/toolkit
```

## 使用步驟

1.設定 provider

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import App from './App';
import store from './store';

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root'),
);
```

2.新增 store

`/store/index.js`

```javascript
import { configureStore } from '@reduxjs/toolkit';
import { combineReducers } from 'redux';
import user from './user';

const reducer = combineReducers({
  user
})
const store = configureStore({
  reducer,
})
export default store;
```

3.新增 redux-toolkit 版本的簡化 reducer

`/store/user.js`

> 這邊記得 `export default slice.reducer` 不是 `slice.reducers`

```javascript
import { createSlice } from '@reduxjs/toolkit'

const slice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    increment: (state, action) => state + action.payload,
    decrement: (state, action) => state - action.payload
  }
})

export default slice.reducer

const { increment, decrement } = slice.actions
export const doIncrement = (num) => async dispatch => {
  // do some async fetch here
  return dispatch(increment(num))
}
```

4.從 component 發送 action

`App.js`

```javascript
import { useDispatch, useSelector } from 'react-redux'
import { doIncrement } from './store/user';

const App = () => {
  const dispatch = useDispatch()
  const user = useSelector(state => state.user)
  
  const handleSave = () => {
    dispatch(doIncrement(2))
  }
  .....省略
```

> 如果用 Class component 可以使用 connect 接入 store state 與 dispatch

```javascript
import { connect } from 'react-redux'

class App extends React.Component {
  // this.props.dispatch
  // this.props.user
}
export default connect((state) => state)(App);
```

{% embed url="https://www.softkraft.co/how-to-setup-redux-with-redux-toolkit/" %}

## Next.js 整合 redux toolkit&#x20;

/pages/\_app.tsx

> 主要增加 \_app.tsx 檔案，其他步驟與上述相同。

```javascript
import { Provider } from 'react-redux';
import type { AppProps } from 'next/app';
import store from '../store/index';

function App({
  Component, pageProps,
}: AppProps) {
  return (
    <Provider store={store}>
      <Component {...pageProps} />
    </Provider>
  );
}

export default App;
```

## 動態 State 內容

與 Redux 原本作法類似：

```javascript
import { createSlice } from '@reduxjs/toolkit'

const slice = createSlice({
  name: 'totalBalance',
  initialState: {},
  reducers: {
    updateUserTotalBalance: (state, action) => ({
      ...state,
      [action.payload.protocolName]: state[action.payload.protocolName] 
        ? state[action.payload.protocolName] + action.payload.num 
        : action.payload.num
    }),
  }
})

export default slice.reducer

const { updateUserTotalBalance } = slice.actions
export const doUpdateUserTotalBalance = (num, protocolName) => async dispatch => {
  // do some async fetch here
  return dispatch(updateUserTotalBalance({ num, protocolName }))
}
```
