# 自己寫一個 React Router

> 1.使用onpopstate管理瀏覽器的前後頁切換
>
> 2.換到下一頁時用`setState` 控制畫面
>
> 3.用 window.history.pushState 推入瀏覽器 URL

```js
import React, { Component } from 'react';

class App extends Component {
  constructor() {
    super();
    this.state = {
      page: window.location.pathname // 重新整理時抓取URL render
    };
    window.onpopstate = () => {  // 點選瀏覽器上一頁時
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
      case '/login':
        return (
          <div>Login Page</div>
        )
      case '/main':
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



