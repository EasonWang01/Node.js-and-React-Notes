# Redux toolkit

可以省去部分步驟，簡單地設定 Redux

```text
yarn add react-redux redux @reduxjs/toolkit
```

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

/store/index.js

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

/store/user.js

> 這邊記得 export default reducer 不是 reducers

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

從 component 發送 action

app.js

```javascript
import { useDispatch, useSelector } from 'react-redux'
import { doIncrement } from './store/user';

const App = () => {
  const dispatch = useDispatch()
  const { user } = useSelector(state => state.user)
  
  const handleSave = () => {
    dispatch(doIncrement(2))
  }
  .....省略
```



{% embed url="https://www.softkraft.co/how-to-setup-redux-with-redux-toolkit/" %}



