---
description: 前言：有關 Redux 基礎概念可以看上一篇
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

## 加入 Typescript&#x20;

如果出現 `(dispatch: any) => Promise` 可以加上如下

1. store.ts 內新增

```javascript
// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch
```

2.component 內 useDispatch 加上 type

```javascript
const dispatch = useDispatch<AppDispatch>();
```

[https://redux.js.org/usage/usage-with-typescript](https://redux.js.org/usage/usage-with-typescript)

## Redux toolkit 結合 localstorage

> 用途：存入 state 到 localstorage，使用 redux-localstorage-simple 套件

store.js

```javascript
import { configureStore } from '@reduxjs/toolkit';
import { combineReducers} from 'redux';
import { save, load } from "redux-localstorage-simple"
import user from './user.js';

const reducer = combineReducers({
  user
})
const store = configureStore({
  reducer,
  middleware: (getDefaultMiddleware) => [
    ...getDefaultMiddleware(),
    save(),
  ],
  preloadedState: load(),
})

export default store;
```

> 如果用 next.js 出現 Hydration failed because the initial UI does not match what was rendered on the server
>
> 可以參考：[https://github.com/vercel/next.js/discussions/35773#discussioncomment-2840696](https://github.com/vercel/next.js/discussions/35773#discussioncomment-2840696)

persist 也可參考官方使用的另一個套件 redux-persist： [https://redux-toolkit.js.org/usage/usage-guide#use-with-redux-persist](https://redux-toolkit.js.org/usage/usage-guide#use-with-redux-persist)
