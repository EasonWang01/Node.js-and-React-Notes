# React router

延續上一章

先`npm install react-router`

接著將我們的client/client.js 改為如下

```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import { Router, Route, hashHistory } from 'react-router'

render(( 
	<Router history={hashHistory}>
	<Route path="/" component={App}/>
     <Route path="/about" component={Proptest}/>
    </Router> 
  ),document.getElementById('app'))


```





參考至

https://github.com/reactjs/react-router-tutorial/tree/master/lessons/02-rendering-a-route

