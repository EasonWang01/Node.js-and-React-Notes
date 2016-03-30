# React router

延續上一章

先`npm install react-router`

1.接著將我們的client/client.js 改為如下

```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import { Router, Route, hashHistory } from 'react-router'

render(( 
	<Router history={hashHistory}>
	<Route path="/" component={App}/>
     <Route path="/Proptest" component={Proptest}/>
     <Route path="/TextDisplay" component={TextDisplay}/>
    </Router> 
  ),document.getElementById('app'))


```
即可看到現在可以再url輸入http://localhost:3000/#/about

進入另一個元件

2.接著到App.js加上

```import { Link } from 'react-router```




參考至

https://github.com/reactjs/react-router-tutorial/tree/master/lessons/02-rendering-a-route

