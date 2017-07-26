# Redux sagas

[https://github.com/redux-saga/redux-saga](https://github.com/redux-saga/redux-saga)

簡介: 在action要放異步動作時，一般Redux的action會錯誤，所以只能用thunk或saga的方法

而saga的概念是監聽action，然後監聽到了之後執行相對應的邏輯，使用ES6 gererator的概念

> 注意: 如果使用saga的put之類的用法但不在generator function內的話可能會錯誤

1.一個按鈕點擊後發出一個普通action

```js
<div onClick={() => userLoginEnter()} />

userLoginEnter() {
    this.props.userLoginEnterAction({
      password: this.state.pass,
      groupId: this.state.gid,
      userId: this.state.name
    });  
}
```

2.該action與reducer

```js
export function userLoginEnter(payload) {
  return {
    payload,
    type: USER_LOGIN_ENTER
  };
}
```

reducer

```js
function loginPageReducer(state = initialState, action) {
  switch (action.type) {
  case USER_LOGIN_ENTER:
    return state.set('login', true);
  case USER_LOGIN:
    sessionStorage.setItem(sessionConst.userInfo, JSON.stringify(action.data));
    return state.set('userInfo', action.data);
  default:
    return state;
  }
}
```

最後寫sagas 的function與 watcher

```js
export function* userLoginEnter(action) {
  try {
    const payload = yield new Promise(resolve => {
      callApi('post', 'login', {
        password: action.payload.password,
        groupId: action.payload.groupId,
        userId: action.payload.userId,
        simpleLogin: false,
        appId: "we6"
      }, (response) => { resolve(response); });
    });
    yield put(userLogin(payload));
  } catch (err) {
    console.error(`userLogin saga error, err msg => ${err}`);
  }
}


export function* watchUserLogin() {
  yield takeLatest(USER_LOGIN_ENTER, userLoginEnter);
}

export default [
  watchUserLogin
];
```

記得要在store.js  run

```js
sagaMiddleware.run(watchUserLogin);
```



