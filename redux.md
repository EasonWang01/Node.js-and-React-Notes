# Redux
source code
https://cdnjs.cloudflare.com/ajax/libs/redux/3.3.1/redux.js
#概念
views點擊=>action => reducer => store =>回傳state給views

1.state統一由store保存，任何更新state都要告知store

2.讓views得到store中state的方法
```
1.使用connect讓最上層元件取得Provider中的store，再用props傳下去

2.使用store.getState
```

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

```
1.定義一個action物件


2.定義reducer(對應不同action型態做不同處理)


3.綁定reducer給createStore


4.view發出dispatch(action) 傳給reducer再更新store


5.Store傳回給view
```
##實做:
1.

package.json
```
{
  "name": "react-todo-list",
  "version": "1.0.0",
  "description": "A simple todo list app built with React, Redux and Webpack",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "nodemon server/server.js --ignore components"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/kweiberth/react-todo-list.git"
  },
  "author": "Kurt Weiberth",
  "license": "ISC",
  "dependencies": {
    "babel-core": "^6.4.5",
    "babel-loader": "^6.2.2",
    "babel-preset-es2015": "^6.3.13",
    "babel-preset-react": "^6.3.13",
    "babel-preset-react-hmre": "^1.1.0",
    "express": "^4.13.4",
    "react": "^0.14.7",
    "react-dom": "^0.14.7",
    "redux-logger": "^2.6.1",
    "webpack": "^1.12.13",
    "webpack-dev-middleware": "^1.5.1",
    "webpack-hot-middleware": "^2.6.4"
  }
}

```
新增後輸入`npm install`

2.

webpack.config.js
```
var webpack = require('webpack');

module.exports = {
  devtool: 'inline-source-map',
  entry: [
    'webpack-hot-middleware/client',
    './client/client.js'
  ],
  output: {
    path: require("path").resolve("./dist"),
    filename: 'bundle.js',
    publicPath: '/'
  },
  plugins: [
    new webpack.optimize.OccurrenceOrderPlugin(),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
  ],
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['react', 'es2015', 'react-hmre']
        }
      }
    ]
  }
}

```
3.

新增server 資料夾，裡面放入server.js
```
var express = require('express');
var path = require('path');
var config = require('../webpack.config.js');
var webpack = require('webpack');
var webpackDevMiddleware = require('webpack-dev-middleware');
var webpackHotMiddleware = require('webpack-hot-middleware');

var app = express();

var compiler = webpack(config);

app.use(webpackDevMiddleware(compiler, {noInfo: true, publicPath: config.output.publicPath}));
app.use(webpackHotMiddleware(compiler));

app.use(express.static('./dist'));

app.use('/', function (req, res) {
    res.sendFile(path.resolve('client/index.html'));
});

var port = 3000;

app.listen(port, function(error) {
  if (error) throw error;
  console.log("Express server listening on port", port);
});

```
4.
  
新client資料夾，裡面放入index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>React Todo List</title>
</head>
<body>
  <div id="app"></div>
  <script src="bundle.js"></script>
</body>
</html>

```
以及，client.js
```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import configureStore from '../redux/store'
import {Provider} from 'react-redux'

let initialState = {
	todos:[{
		id:0,
		completed: false,
		text:'initial for demo'

	}]
}
let store = configureStore(initialState);

render(
  <Provider store={store} >
  <App/>
  </Provider>,
  document.getElementById('app')
)

```
Provider用來連結react即redux的store


5.

新增redux資料夾

裡面放入三個檔案

`store.js`  `action.js`  `reducer.js`

store.js

```
import {applyMiddleware,compose,createStore} from "redux"
import reducer from './reducer'
import logger from 'redux-logger'

let finalCreateStore = compose(
	applyMiddleware(logger())
	)(createStore)

export default function configureStore(initialState = { todos:[]}){
	return finalCreateStore(reducer,initialState)

}
```
這裡我們加入了中間件logger

而store的必要參數為reducer，我們用`import reducer from './reducer'`傳入

action.js
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
reducer.js
```
let getId = 1 ;

export default function reducer(state,action){
	switch(action.type){
		case 'ADD_TODO':
			
			return(	Object.assign({},state,{
				todos:[{
				  text:action.text,
				  completed:false,
				  id:getId++

				},...state.todos]
			})
			)
		default:
			return state;

	}

}
```
最後建立component資料夾

裡面放入`App.js` `TodoInput.js`  `TodoList.js`

App.js
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
從chrome React dev tool可以看到如果使用connect

App元件可以接到從store來的state且轉成他的props

TodoInput.js
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
class TodoInput extends Component {

  constructor(props, context) {
    super(props, context)
 
    this.handleSubmit = this.handleSubmit.bind(this);
  }


 
  handleSubmit(){
    event.preventDefault()
    //this.props.dispatch()
    console.log(this._input.value);
    this.props.dispatch(action.addTodo(this._input.value));
  }




  render() {
    return (
      <div>
        <input
          type="text"

          placeholder="Type in your tode"
     
          ref={(c) => this._input = c}
        />
        <button onClick={this.handleSubmit}>Submit</button>
      </div>
    )
  }

}

export default TodoInput

```
TodoList.js
```
import React, { Component } from 'react'

class TodoList extends Component {

  render() {
    return (
      <ul>
        {
          this.props.todos.map((todo)=>{
            return <li key={todo.id}> {todo.text}  </li>
          })
        }

      </ul>
    )
  }

}

export default TodoList

```
完整版:(於master branch)

https://github.com/EasonWang01/Redux-tutorial 

#接著幫他加上toggle
使點選後判斷完成了沒，來讓事項有一橫線

1.先在action.js
```
let  actions ={ 
	addTodo:(text)=>{
		return ({
	type:'ADD_TODO',
	text:text})
},
	toggleTodo:(id)=>{
		return({
	type:'TOGGLE_TODO',
	id:id,
	})

	}
}


export default actions
```
2.reducer.js

```
let getId = 1 ;

export default function reducer(state,action){
	switch(action.type){
		case 'ADD_TODO':
			
			return(	Object.assign({},state,{
				todos:[{
				  text:action.text,
				  completed:false,
				  id:getId++

				},...state.todos]
			})
			)
		case 'TOGGLE_TODO':

      return Object.assign({},state,{todos:state.todos.map(function(state){
                if(state.id!==action.id){
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) }
      )


	

				

		default:
			return state;

	}

}
```
最後在TodoList.js
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
class TodoList extends Component {

   constructor(props, context) {
    super(props, context)
 
  }





  liClick(a){
   
      store.dispatch(action.toggleTodo(a.id));

  }




  render() {
    return (
      <ul>
        {
          this.props.todos.map((todo)=>{
            return <li 
            key={todo.id} 
            onClick={()=>this.liClick(todo)} 
            style= {{textDecoration:todo.completed?'line-through':'none'}}  
            >
             {todo.text}  

             </li>
          })
        }

      </ul>
    )
  }

}

export default TodoList

```
完整版在 branch toggle

#接著加入三個選項，分別顯示all,active,completed

1.新增FilterLink.js
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
class FliterLink extends Component {

	render(){


	return	<a  href='#'
			onClick={e=>{
				e.preventDefault();
				//console.log(this.props.filter)
				store.dispatch(action.FilterTodo(this.props.filter))
										
			}}
					
		>
			{this.props.children}
		</a>
	}



}

export default FliterLink
```
2.將他加入TodoList.js

```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
import FilterLink from './FilterLink.js'

class TodoList extends Component {

   constructor(props, context) {
    super(props, context)
 
  }





  liClick(a){
   
      store.dispatch(action.toggleTodo(a.id));

  }




  render() {
    return (
      <div>
      <ul>
        {
          this.props.todos.map((todo)=>{
            return <li 
            key={todo.id} 
            onClick={()=>this.liClick(todo)} 
            style= {{textDecoration:todo.completed?'line-through':'none'}}  
            >
             {todo.text}  

             </li>
          })
        }

      </ul>
      <p>
          {"Show: "}
          
          <FilterLink filter="SHOW_ALL">
         All
          </FilterLink>
          {"  ,  "}
          <FilterLink filter="SHOW_ACTIVE">
         Active
          </FilterLink>
          {"  ,  "}
          <FilterLink filter="SHOW_COMPLETED">
         Completed
          </FilterLink>

      </p>
      </div>
    )
  }

}

export default TodoList


```
更改action.js
```
let  actions ={ 
	addTodo:(text)=>{
		return ({
	type:'ADD_TODO',
	text:text})
},
	toggleTodo:(id)=>{
		return({
	type:'TOGGLE_TODO',
	id:id,
	})

	},
	FilterTodo:(filter)=>{
		return({
	type:'SET_VISBILITY_FILTER',
	filter:filter		

		})
	}
}


export default actions
```
以及reducer.js
```
let getId = 1 ;

export default function reducer(state,action){
	switch(action.type){
		case 'ADD_TODO':
			
			return(	Object.assign({},state,{
				todos:[{
				  text:action.text,
				  completed:false,
				  id:getId++

				},...state.todos]
			})
			)
		case 'TOGGLE_TODO':

      return Object.assign({},state,{todos:state.todos.map(function(state){
                if(state.id!==action.id){
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) }
      )

      	case 'SET_VISBILITY_FILTER':

      	return state
	

				

		default:
			return state;

	}

}
```

(此時多了三個選項，且點擊會發出action)

3.接著我們幫他加入發出action後所要做的事

我們會使用array的filter方法，過濾出state.todos中completed為false的方法

(給點擊active按鈕用)反之為給completed按鈕用
```
var filtered = (this.props.todos).filter(function(state){
  return state.completed==false

});
console.log(filtered)
```
(filter()讓array中每個元素接受一個function的運算且return一個值，為true或false)
(如果為true則繼續保留在array)

但

在這之前

我們先幫reducer加上一個狀態
```
let getId = 1 ;

export default function reducer(state,action){
	switch(action.type){
		case 'ADD_TODO':
			
			return(	Object.assign({},state,{
				todos:[{
				  text:action.text,
				  completed:false,
				  id:getId++

				},...state.todos]
			})
			)
		case 'TOGGLE_TODO':

      return Object.assign({},state,{todos:state.todos.map(function(state){
                if(state.id!==action.id){
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) }
      )

      	case 'SET_VISBILITY_FILTER':

      	return Object.assign({},state,{visbility:action.filter}) 
	

				

		default:
			return state;

	}

}
```
接著將App.js改為
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
        <TodoInput />
        <TodoList  todos={this.props}/>
     
  
      </div>
    )
  }

}
function  mapStateToProp(state){

	return state
}


export default connect(mapStateToProp)(App)

```
(因為我們現在state裡不只一個state，所以先傳入整包，於子代再做取出)

最後

(把原本傳入最後return要map的物件先做過濾)


TodoList.js

```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
import FilterLink from './FilterLink.js'

class TodoList extends Component {

   constructor(props, context) {
    super(props, context)
 
  }





  liClick(a){
   
      store.dispatch(action.toggleTodo(a.id));

  }




  render() {
   
    var filtered  = function(){
       
      switch(this.props.todos.visbility){

      case "SHOW_ALL":
        return this.props.todos.todos;

      case "SHOW_ACTIVE":
        return (this.props.todos.todos).filter(function(state){
                    return state.completed==false

               });

      case "SHOW_COMPLETED":
        return (this.props.todos.todos).filter(function(state){
                    return state.completed==true

               });
      default:
        return this.props.todos.todos;
      }
    }.bind(this)
    
   
    return (
      <div>
      <ul>
        {
          filtered().map((todo)=>{
            return <li 
            key={todo.id} 
            onClick={()=>this.liClick(todo)} 
            style= {{textDecoration:todo.completed?'line-through':'none'}}  
            >
             {todo.text}  

             </li>
          })
        }

      </ul>
      <p>
          {"Show: "}

          <FilterLink filter="SHOW_ALL">
         All
          </FilterLink>
          {"  ,  "}
          <FilterLink filter="SHOW_ACTIVE">
         Active
          </FilterLink>
          {"  ,  "}
          <FilterLink filter="SHOW_COMPLETED">
         Completed
          </FilterLink>

      </p>
      </div>
    )
  }

}

export default TodoList

```
使用bind是因為我們在function內想取得class的執行環境，所以把他綁到this

3.接著

讓點擊後的link變黑色，無法重複點擊

所以先幫FilterLink加上一個props
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
import FilterLink from './FilterLink.js'

class TodoList extends Component {

   constructor(props, context) {
    super(props, context)
 
  }





  liClick(a){
   
      store.dispatch(action.toggleTodo(a.id));

  }




  render() {
   
    var filtered  = function(){
       
      switch(this.props.todos.visbility){

      case "SHOW_ALL":
        return this.props.todos.todos;

      case "SHOW_ACTIVE":
        return (this.props.todos.todos).filter(function(state){
                    return state.completed==false

               });

      case "SHOW_COMPLETED":
        return (this.props.todos.todos).filter(function(state){
                    return state.completed==true

               });
      default:
        return this.props.todos.todos;
      }
    }.bind(this)
   
   
/*
var filtered = (this.props.todos).filter(function(state){
  return state.completed==false

});*/
//console.log(filtered)

    return (
      <div>
      <ul>
        {
          filtered(
            ).map((todo)=>{
            return <li 
            key={todo.id} 
            onClick={()=>this.liClick(todo)} 
            style= {{textDecoration:todo.completed?'line-through':'none'}}  
            >
             {todo.text}  

             </li>
          })
        }

      </ul>
      <p>
          {"Show: "}

          <FilterLink filter="SHOW_ALL"currentFilter={this.props.todos.visbility}>
         All
          </FilterLink>
          {"  ,  "}
          <FilterLink filter="SHOW_ACTIVE"currentFilter={this.props.todos.visbility}>
         Active
          </FilterLink>
          {"  ,  "}
          <FilterLink filter="SHOW_COMPLETED"currentFilter={this.props.todos.visbility}>
         Completed
          </FilterLink>

      </p>
      </div>
    )
  }

}

export default TodoList

```
之後點擊link後去偵測，這個link的filter props和當前store的filter屬性如符合的話，則只回傳普通的文字
```
import React, { Component } from 'react'
import action from '../redux/actions.js'
import store from '../redux/store'
class FliterLink extends Component {

	render(){
		if(this.props.currentFilter==this.props.filter){
			return <span> {this.props.children}</span>
		}

	return	<a  href='#'
			onClick={e=>{
				e.preventDefault();
				//console.log(this.props.filter)
				store.dispatch(action.FilterTodo(this.props.filter))
										
			}}
					
		>
			{this.props.children}
		</a>
	}



}

export default FliterLink
```
#使用combined reduecer

在這之前，我們先改變我們reducer的寫法

```javascript

let getId = 1 ;

function todos(state,action){
	switch(action.type){
		case 'ADD_TODO':
			
			return [{
				  text:action.text,
				  completed:false,
				  id:getId++

				},...state.todos]
			
		case 'TOGGLE_TODO':

      return state.todos.map(function(state){
                if(state.id!==action.id){
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) 

		default:
			return state.todos;

	}

}



function visbility(state,action){
	switch(action.type){
		case 'SET_VISBILITY_FILTER':

      	return action.filter
	

		default:
			return state.visbility;
 }
}





function reducer (state,action){
  return	Object.assign({},state,{
				visbility:visbility(state,action),
				todos:todos(state,action)

	});

}


export default reducer
```
(上面，我們將reducer寫成兩個function)

combined reducer即是這個概念，所以我們把reducer.js 改為如下
```javascript
import { combineReducers } from 'redux'
let getId = 1 ;

function todos(state=[],action){
	switch(action.type){
		case 'ADD_TODO':
			
			return [{
				  text:action.text,
				  completed:false,
				  id:getId++

				},...state]
			
		case 'TOGGLE_TODO':

      return state.map(function(state){
                if(state.id!==action.id){
                return  state
                };

                return {...state,completed:!state.completed}
           
			}) 

		default:
			return state;

	}

}



function visbility(state="SHOW_ALL",action){
	switch(action.type){
		case 'SET_VISBILITY_FILTER':

      	return action.filter
	

		default:
			return state;
 }
}



const rootReducer = combineReducers({
  visbility: visbility,
  todos:todos
})


export default rootReducer
```
兩個重點
```
1. combineReducers 沒有丟預設state進去所以，我們要用ES6的寫法幫function寫上預設參數(Line 4 and 35)
2. function內一開始傳進去的的不再是整個state而是取key後的state，所以裡面也須更改(Line 13. 27.17.43)
```
最後，在ES6在物件的key跟value名稱相同時可省略

所以寫成
```
const rootReducer = combineReducers({
  visbility,
  todos,
})
```
>在多人合作中，我們會把reducer每個function放在不同檔案，最後再import到rootReducer來減少多人一起開發功能時的conflict

#中間件ActionCreator

原本App.js的props裡只有Provider傳下來的dispatch方法，而使用這個後，dispatch被取代為actions

App.js加上
```
import { bindActionCreators } from 'redux';
import actions from '../redux/actions.js'

```
```
function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(actions, dispatch)
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(App)
```
即可發現點選React devtool中App的props的dispatch變為actions物件

1.

原本父元件使用`this.props.dispatch`將dispatch方法傳下去，現在改為用`this.props.actions`

2.

而VIEW發送ACTION的方式從
```
  handleSubmit(event) {
    event.preventDefault()
    this.props.dispatch(actions.addTodo(this.state.inputText))
  }
```
改為
```
  handleSubmit(event) {
    event.preventDefault()
    this.props.addTodo(this.state.inputText)
  }
```

但

如果你是直接把STORE在每個元件引入的話，每個元件就直接知道有DISPATCH這個方法，所以也不用往下傳遞PROPS教導，所以也不用使用ACTIONCreator這個方法

參考:https://camsong.github.io/redux-in-chinese/docs/api/bindActionCreators.html


#操作非同步動作(Async)
例如:
>我們今天有一個按鈕，點擊後發出action去跟server要資料，回傳資料後再觸發一個action去更新state

###使用Redux-thunk 

`npm install redux-thunk`

將store.js改為
```
import {applyMiddleware,compose,createStore} from "redux"
import reducer from './reducer'
import logger from 'redux-logger'
import thunk from 'redux-thunk'

let initialState = {
	visbility:'SHOW_ALL',
	todos:[{
		id:0,
		completed: false,
		text:'initial for demo'

	}]
}


let finalCreateStore = compose(
	applyMiddleware(thunk,logger())
	)(createStore)

function configureStore(initialState){
	return finalCreateStore(reducer,initialState)

}

let store = configureStore(initialState)

export default store
```
之後我們action.js裡面寫的方法不只可以return物件，還可以return function

這個function就是用來寫非同步API，最後拿回資料再寫return，之後即會送到reducer處理

例如:
```
	FilterTodo:(filter)=>{
		return({
	type:'SET_VISBILITY_FILTER',
	filter:filter		

		})
	},
///上面是一般的action，下面是thunk的action

	con:()=>{
		return (dispatch,getState)=>{
			console.log(getState())
		}
	}
```
在action.js內可以直接調用store的所有方法
