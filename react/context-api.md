# Context API

## Context API

React 16.3後新增了Context API，可以用來替代Redux。

官方文件:

> [https://reactjs.org/docs/context.html](https://reactjs.org/docs/context.html)

不錯的文章:

> [https://medium.freecodecamp.org/replacing-redux-with-the-new-react-context-api-8f5d01a00e8c](https://medium.freecodecamp.org/replacing-redux-with-the-new-react-context-api-8f5d01a00e8c)

可參考此範例:

> [https://github.com/EasonWang01/React-context-api-boilerplate](https://github.com/EasonWang01/React-context-api-boilerplate)

## 範例:

App.js

```javascript
import React, { PureComponent, createContext } from 'react';
import {
  BrowserRouter as Router,
  Route,
  Link,
  Redirect,
} from 'react-router-dom'

// Initialize a context
const Context = createContext()

// This context contains two interesting components
const { Provider, Consumer } = Context

class App extends PureComponent {
  constructor() {
    super();
    this.state = {
      user: 'test1231'
    }
  }

  render() {
    return (
      <Router>
        <Provider value={{
          state: this.state,
          actions: {
            increment: () => this.setState({ count: this.state.count + 1 }),
          }
        }}>
          <Route path="/login" render={() => <Login Consumer={Consumer} />}  />
          <Route path="/register" component={Register} />
          <Route path="/reset" component={ResetPass} />
          <Route path="/lobby" component={Lobby} />
        </Provider>
      </Router>
    );
  }
}

export default App;
```

Login.js

```javascript
import React, { PureComponent } from 'react';

export default class Login extends PureComponent {
  render() {
    const Consumer = this.props.Consumer;
    return (
      <Consumer>
        {({ state, actions }) => (
          <div>
            <div>{state.user}</div>
          </div>
        )}
      </Consumer>
    )
  }
}
```

### 步驟:

1.使用Provider提供根組件，任何包在Provider內的子組件均可使用Consumer來存取根組件數據。

```javascript
const Context = createContext()

const { Provider, Consumer } = Context
```

2.在react-router把consumer傳給元件。

3.在元件內使用Consumer包住所有dom，dom內即可存取provider的數據。

## 相關框架

> [https://github.com/didierfranc/react-waterfall](https://github.com/didierfranc/react-waterfall)
>
> [https://github.com/elisherer/react-waterfall-redux-devtools-middleware](https://github.com/elisherer/react-waterfall-redux-devtools-middleware)

