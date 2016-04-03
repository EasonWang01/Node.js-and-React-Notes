# Redux
source code
https://cdnjs.cloudflare.com/ajax/libs/redux/3.3.1/redux.js
#概念
action => reducer => store

#簡單範例
```
<!DOCTYPE html>
<html>
  <head>
    <title>Redux basic example</title>
    <script src="https://npmcdn.com/redux@latest/dist/redux.min.js"></script>
  </head>
  <body>
    <div>
      <p>
        Clicked: <span id="value">0</span> times
        <button id="increment">+</button>
        <button id="decrement">-</button>
        <button id="incrementIfOdd">Increment if odd</button>
        <button id="incrementAsync">Increment async</button>
      </p>
    </div>
     </body>
</html>
```
```
 <script>
    
      var store = Redux.createStore(counter)
      var valueEl = document.getElementById('value')
      function render() {
        valueEl.innerHTML = store.getState().toString()
      }
      render()
      store.subscribe(render)
     
    </script>
```
Reducer
```

  function counter(state, action) {
        if (typeof state === 'undefined') {
          return 0
        }
        switch (action.type) {
          case 'INCREMENT':
            return state + 1
          case 'DECREMENT':
            return state - 1
          default:
            return state
        }
      }
```
```
 document.getElementById('increment')
        .addEventListener('click', function () {
          store.dispatch({ type: 'INCREMENT' })
        })
      document.getElementById('decrement')
        .addEventListener('click', function () {
          store.dispatch({ type: 'DECREMENT' })
        })
      document.getElementById('incrementIfOdd')
        .addEventListener('click', function () {
          if (store.getState() % 2 !== 0) {
            store.dispatch({ type: 'INCREMENT' })
          }
        })
      document.getElementById('incrementAsync')
        .addEventListener('click', function () {
          setTimeout(function () {
            store.dispatch({ type: 'INCREMENT' })
          }, 1000)
        })
```

#使用React連結Redux
https://github.com/reactjs/redux/tree/master/examples/counter

建立counter的元件(原本的HTML)
```
import React, { Component, PropTypes } from 'react'

class Counter extends Component {
  constructor(props) {
    super(props)
    this.incrementAsync = this.incrementAsync.bind(this)
    this.incrementIfOdd = this.incrementIfOdd.bind(this)
  }

  incrementIfOdd() {
    if (this.props.value % 2 !== 0) {
      this.props.onIncrement()
    }
  }

  incrementAsync() {
    setTimeout(this.props.onIncrement, 1000)
  }

  render() {
    const { value, onIncrement, onDecrement } = this.props
    return (
      <p>
        Clicked: {value} times
        {' '}
        <button onClick={onIncrement}>
          +
        </button>
        {' '}
        <button onClick={onDecrement}>
          -
        </button>
        {' '}
        <button onClick={this.incrementIfOdd}>
          Increment if odd
        </button>
        {' '}
        <button onClick={this.incrementAsync}>
          Increment async
        </button>
      </p>
    )
  }
}

Counter.propTypes = {
  value: PropTypes.number.isRequired,
  onIncrement: PropTypes.func.isRequired,
  onDecrement: PropTypes.func.isRequired
}

export default Counter
```
render到html
```
import React from 'react'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import Counter from './components/Counter'
import counter from './reducers'

const store = createStore(counter)
const rootEl = document.getElementById('root')

function render() {
  ReactDOM.render(
    <Counter
      value={store.getState()}
      onIncrement={() => store.dispatch({ type: 'INCREMENT' })}
      onDecrement={() => store.dispatch({ type: 'DECREMENT' })}
    />,
    rootEl  
  )
}

render()
store.subscribe(render)
```
最後定義reducer
```
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```
完成

----
上例我們沒有直接去定義action.js而是直接指定action的type

我們可改寫成
```
      onIncrement={() => store.dispatch(increment(text))}
      onDecrement={() => store.dispatch(decrement(text))}
```
action.js
```
function increment(text) {
  return {
    type: INCREMENT,
    //text
  };
}
function decrement(text) {
  return {
    type: DECREMENT,
    //text
  };
}
```
#另一個稍為更詳細的範例

###流程:


1.定義一個action物件
```
let  actions ={ 
	addTodo:(text)=>{
		return ({
            type:'ADD_TODO',
            text:text})
  }
}


export default actions
```
2.定義reducer(對應不同action型態做不同處理)

```
function getId(state){
	return state.todos.reduce((maxId,todo)=>{
		return Math.max(todo.id,maxId)
	},-1) + 1

}


export default function reducer(state,action){
	switch(action.type){
		case 'ADD_TODO':
			
			return(	Object.assign({},state,{
				todos:[{
				  text:action.text,
				  completed:false,
				  id:getId(state)

				},...state.todos]
			})
			)
		default:
			return state;

	}

}
```

3.綁定reducer給createStore
```
import {applyMiddleware,compose,createStore} from "redux"
import reducer from './reducer'
import logger from 'redux-logger'
//下面為，使我們再console可以看到action發出的logger
let finalCreateStore = compose(
	applyMiddleware(logger())
	)(createStore)

export default function configureStore(initialState = { todos:[]}){
	return finalCreateStore(reducer,initialState)

}

```
4.view發出dispatch(action) 傳給reducer再更新store
```
import React, { Component } from 'react'
import TodoInput from './TodoInput.js'
import TodoList from './TodoList.js'
import {connect} from 'react-redux'
class App extends Component {

  render() {
    return (
      <div>
        <h1>Todo list</h1>
        <TodoInput dispatch={this.props.dispatch}/>
        <TodoList dispatch={this.props.dispatch} todos={this.props.todos}/>
      </div>
    )
  }

}
function  mapStateToProps(state){

	return state
}


export default connect(mapStateToProps)(App)


```

5.Store傳回給view
```
使用connect後connect擁有store (顯示為state)

使用connect(mapStateToProps)(App)  將connect的store傳入App元件 為其props
```
