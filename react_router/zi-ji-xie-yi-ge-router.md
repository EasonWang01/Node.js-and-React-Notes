# 自己寫一個 React Router

```js
import React, { Component } from 'react';

class App extends Component {
  constructor() {
    super();
    this.state = {
      page: '/loginPage'
    };
    window.onpopstate = () => {
      this.handleRoute(document.location.pathname, true);
    }
  }
  handleRoute(path, pop) {
    if (!pop) window.history.pushState({}, "", path);
    this.setState({ page: path });
  }
  handlelogin() {
    this.handleRoute('/mainPage');
  }
  render() {
    switch (this.state.page) {
      case '/':
      case '/loginPage':
        return (
          <div>Login Page</div>
        )
      case '/mainPage':
        return (
          <div>Main Page</div>
        )
      default:
        break;
    }
  }
}

export default App;
```



