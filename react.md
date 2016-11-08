# React
基礎

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
  <script src="https://fb.me/react-15.0.0.js"></script>
    <script src="https://fb.me/react-dom-15.0.0.js"></script>
   <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-
    core/5.8.34/browser.min.js"></script>
</head>
<body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
	
</body>
</html>
```






```
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>;
  }
});

ReactDOM.render(<HelloMessage name="John" />, mountNode);
```
使用ES6 
```
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

ReactDOM.render(<HelloMessage name="Sebastian"/>, mountNode);
```
其他方法之簡單整理
```

class baseComponent extends React.Component{
    // 建構子
    constructor(props){
        super(props);
    }
    // ...其他公用代碼,方法等封装
}    
//定義另一類別，繼承baseComponent
class App extends baseComponent{
    // 建構子
    constructor(props){
        super(props);
    }
    // 初始化state,替代原getInitialState, 注意前面
    沒有static
    state = {
        showMenu:false
    };
    // 替代原propTypes 属性,注意前面有static,屬於靜態方法.
    static propTypes = {
        autoPlay: React.PropTypes.bool.isRequired
    }
    // 默認defaultProps,替代原getDefaultProps方法, 注意前面
    有static
    static defaultProps = {
        loading:false
    };
    // 這以用箭頭函數箭頭函數不會改變this的指向,否則函數內,
    this指的就不是當前對象了
    // React.CreatClass方式React會自動綁定this,ES6寫法不會.
    handleClick = (e)=>{
        this.setState();//
    };
    componentDidMount() {
        // React内置的周期 ,這裡要顯示所繼承父類的相同function,
        // 否則一旦父類中有封裝,子類會把和父類相同的function覆蓋,
        不會執行父類的function.
        // 但...父類如果本身沒有寫出componentDidMount,在子類
        寫出的話就會錯誤,
        
        if (super.componentDidMount) {
            super.componentDidMount();
        }
        // do something yourself...
    }
}

```


更多有關ES5 react to ES6 or ES7
http://cheng.logdown.com/posts/2015/09/29/converting-es5-react-to-es6

http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8

有關class用法
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes

http://es6.ruanyifeng.com/#docs/class

https://gist.github.com/sebmarkbage/d7bce729f38730399d28
#開始使用React
##建立環境

npm install webpack -g

npm install nodemon -g
(在更改程式時自動執行server，而forever為遇到錯誤也不會停止)

1.裡面放入package.json
```
{
  "name": "react-todo-list",
  "version": "1.0.0",
  "description": "A simple todo list app built with React, Redux and Webpack",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "nodemon server/server.js"
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
    "express": "^4.13.4",
    "react": "^0.14.7",
    "react-dom": "^0.14.7",
    "webpack": "^1.12.13"
  }
}
```
之後輸入npm install

在根目錄下新建三個目錄
```
forclass
    --client
    --components
    --server
    package.json
 ```
 2.接著在server目錄下新增server.js
 ```
 var express = require('express');
var path = require('path');

var app = express();

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
3.在client資料夾內加入index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>React Todo List</title>
</head>
<body>
  <h1>This is not a React app yet!</h1>
  <div id="app"></div>
  <script src="bundle.js"></script>
</body>
</html>
```
4.新增webpack 配置文件webpack.config.js
```
module.exports = {
  devtool: 'inline-source-map',
  entry: ['./client/client.js'],
  output: {
    path: './dist',
    filename: 'bundle.js',
    publicPath: '/'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['react', 'es2015']
        }
      }
    ]
  }
}
```
什麼是source map

可以看chrome dev tool 裡的setting即有此選項
http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html

5.在client資料夾中新增client.js
```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'

render(
  <App/>,
  document.getElementById('app')
)
```
`<App/>`即為我們的react元件

6.在components資料夾中新增App.js

此即為我們第一個react元件
```
import React, { Component } from 'react'

class App extends Component {

  render() {
    return <div>I'm Banana!</div>
  }

}

export default App
```
輸入`webpack --config webpack.config.js`
會自動產生dist資料夾，裡面包含bundle.js檔案

之後即可重新啟動伺服器，並觀看改變
`npm run serve`(寫在package.json中的scripts內)


##讓我們不用重新整理網頁
在package.json內加入

1.不用重新整理網頁
(讓我們不用使用webpack-dev-server也有-hot的指令)
```
"webpack-hot-middleware": "^2.6.4"
```
2.讓hot middleware知道react的class
```
 "babel-preset-react-hmre": "^1.1.0",
```
以及上webpack跑在我們架設的express server上
```
 "webpack-dev-middleware": "^1.5.1"
```
完整版
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
    "webpack": "^1.12.13",
    "webpack-dev-middleware": "^1.5.1",
    "webpack-hot-middleware": "^2.6.4"
  }
}
```
npm install後

接著更改剛才server資料夾下的 server.js

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
現在我們可以直接用server.js去compile 我們的webpack config檔案，不用再輸入指令compile

最後因為我們剛才有用hot middle所以我們可以使用--hot去讓他自動reload網頁，但我們不想在指令輸入，所以可以把他加在webpack config內

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
現在執行
`npm run serve`

再去更改app.js內的字，可以看到不用重新啟動伺服器，也不用按網頁的重新整理，即可更新

##第二階段
開始新增其他react元件

在components下，新增一個檔案
`TextDisplay.js`
```
import React, {Component} from 'react'

class TextDisplay extend Component{

}

export default TextDisplay
```
上面是引用react後建造一個空的class後將他輸出

接著我們要在class內寫入東西

```
import React, {Component} from 'react'
//JSX要看到import了React 才可以編譯 
class TextDisplay extends Component{

	render() {
		return (
			<div>
			<div> THis is text display</div>
			<div> if we have two div we need to wrap it.</div>
			</div>

		)};

}

export default TextDisplay
```
使著將最外層的div刪掉，會出現錯誤，因為一個元件要有東西包住最外層。

之後讓原來的App.js引用他
```
import React, { Component } from 'react'

class App extends Component {

  render() {
    return (
    <div>
    <div>This is definitely a React app now!</div>
    <TextDisplay/>
    </div>
  )}

}
export default App


```
使用state

TextDisplay.js
```
import React, { Component } from 'react'


class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdxt'
    }
  }



  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="This is going to be text"
          value={this.state.inputText}
         
        />
    
      </div>
    )
  }

}

export default TextInput
```
####!!記得重新整理網頁，才會作用(因為這裡是constructor)

#為元件加入方法
```
import React, { Component } from 'react'


class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdsxt'
    }
  }

 handleChange(){
 	console.log("ch")
 }

  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="This is going to be text"
          value={this.state.inputText}
          onChange={this.handleChange}
        />
    
      </div>
    )
  }

}

export default TextInput
```
##在class中的方法如果有this的話他會不知道this是什麼，所以要在class 的constructor中把該方法綁進來
1.
```
import React, { Component } from 'react'


class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdsxt'
    }
    this.handleChange = this.handleChange.bind(this);
  }

 handleChange(){
 	this.setState({inputText:12});
 }

  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="This is going to be text"
          value={this.state.inputText}
          onChange={this.handleChange}
        />
    
      </div>
    )
  }

}

export default TextInput
```
但後來發現如果想傳入參數還是要在html tag中寫bind才會傳入


2.所以另一種寫法，是直接在DOM 的onchange中綁，但官方推薦綁在constructor
```
onChange={this.handleChange.bind(this)}
```
接著在render上面寫
```
handleChange(){
 ...
}
```


3.第三種寫法(ES6的箭頭函數，最方便，因為會直接幫你綁定)


```
send = () => {
    console.log(this.inputFiled.value)
    let text = this.inputFiled.value;
    this.props.addTodo1(text);
  }


 <button onClick={()=>this.handleSubmit()}>Submit</button>
```

如要傳入事件，記得兩邊()都要傳入
```
 <form onSubmit={(e)=>this.handleSubmit(e)}>
```
好處是不用再用bind

參考:http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html

####!每次改動constructor記得都要重新整理，就算有用Hot reload


完整範例：

`注意其中的onclick 與 從子元件傳上來的 onclick`

container
```
import React, { Component } from 'react'
import {connect} from 'react-redux'
import actions from '../redux/actions/todoActions.js'
import { bindActionCreators } from 'redux'
import List from '../components/List.js'

class TodoList extends Component {
  send = () => {
    let text = this.inputFiled.value;
    this.props.addTodo1(text);
  }
  itemClick = (e,id) => {
    console.log(e)
    console.log(id)
  }
  render() {
    return (
      <div>
        <input ref={(c) => this.inputFiled = c} />
        <button onClick={()=>this.send()}></button>
        <List list={this.props} itemClick={(e,id)=>this.itemClick(e,id)}>
        </List>
      </div>
    )
  }

}
function mapStateToProp(state){
	return state
}

function mapDispatchToProps(dispatch) {
  return bindActionCreators({
    addTodo1:actions.addTodo
  },dispatch);
}

export default connect(mapStateToProp,mapDispatchToProps)(TodoList)



```
component

```
import React from 'react'
const List = (props) => {
 let todos = Array.from(props.list.todos);
  return (
    <div>
      {todos.map( i =>
         <p key={i.id} onClick={(e)=>props.itemClick(e,i.id)}>{i.text}</p>
      )}
    </div>
  )
}

export default List;

```

#2.在class內所有的this都是指到那個class

所以要取得onchange時input內的value必須用e.target
，因為這裡不是DOM
```
import React, { Component } from 'react'


class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdst'
    }
    this.handleChange = this.handleChange.bind(this);
  }

 handleChange(e){
 	console.log(e.target.value);
 	console.log(this);
 	//this.setState({inputText:12});
 }

  render() {
    return (
      <div>
        
    	<input onChange={this.handleChange} />
      </div>
    )
  }

}

export default TextInput
```
#Prop
即為HTML tag中的屬性

1.新增一個元件為Propest.js
```
import React, { Component } from 'react'

class Proptest extends Component {
	render(){
		return <div> {this.props.text}</div>
	}
}
export default Proptest
```
TextDisplay.js
```
import React, { Component } from 'react'
import Proptest from "./Proptest"

class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdst'
    }
    this.handleChange = this.handleChange.bind(this);
  }

 handleChange(e){
 	console.log(e.target.value);
 	
 	this.setState({inputText:e.target.value});
 }

  render() {
    return (
      <div>
        
    	<input onChange={this.handleChange} />
    	<Proptest text="123"/>
      </div>
    )
  }

}

export default TextInput
```
即可看到Proptest的props顯示出

2.讓子代的view啟動父代的method

Proptest.js
```
import React, { Component } from 'react'

class Proptest extends Component {

	constructor(){
		super()
		
	}


	render(){
		return( 

		<div> 
			<button onClick={this.props.deleteLetter}> </button>

		</div>
	)}
}
export default Proptest
```
TestDisplay.js
```
import React, { Component } from 'react'
import Proptest from "./Proptest"

class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdst'
    }
    this.handleChange = this.handleChange.bind(this);
    this.deleteLetter = this.deleteLetter.bind(this);
  }

 handleChange(e){
 	console.log(e.target.value);
 	
 	this.setState({inputText:e.target.value});
 }
 deleteLetter(){
 	console.log(this);
 }

  render() {
    return (
      <div>
        
    	<input onChange={this.handleChange} />
    	<Proptest deleteLetter={this.deleteLetter}/>
      </div>
    )
  }

}

export default TextInput
```

進階(點擊button更改state)
```
import React, { Component } from 'react'
import Proptest from "./Proptest"

class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdst'
    }
    this.handleChange = this.handleChange.bind(this);
    this.deleteLetter = this.deleteLetter.bind(this);
  }

 handleChange(e){
 	console.log(e.target.value);
 	
 	this.setState({inputText:e.target.value});
 }
 deleteLetter(){
 	this.setState({
 		inputText:this.state.inputText.substring(0,this.state.inputText.length-1)
 	});
 }

  render() {
    return (
      <div>
        
    	<input    value={this.state.inputText}     onChange={this.handleChange} />
    	<Proptest text={this.state.inputText} deleteLetter={this.deleteLetter}/>
      </div>
    )
  }

}

export default TextInput
```

```
import React, { Component } from 'react'

class Proptest extends Component {

	constructor(){
		super()
		
	}


	render(){
		return( 

		<div> 
			<p>{this.props.text}</p>
			<button onClick={this.props.deleteLetter}> </button>

		</div>
	)}
}
export default Proptest
```

#在元件內使用條件判斷
```
import React, { Component } from 'react'
import Proptest from "./Proptest"



class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdst'
    }
    this.handleChange = this.handleChange.bind(this);
    this.deleteLetter = this.deleteLetter.bind(this);
  }

 handleChange(e){
 	console.log(e.target.value);
 	
 	this.setState({inputText:e.target.value});
 }
 deleteLetter(){
 	this.setState({
 		inputText:this.state.inputText.substring(0,this.state.inputText.length-1)
 	});
 }






  render() {
  	var checkFalse = true;
  	if(checkFalse){
  		checkFalse =  <Proptest text={this.state.inputText} deleteLetter={this.deleteLetter}/>;
 	}else{
  		checkFalse = <p>This is false</p>
  	}



    return (
      <div>
        
    	<input    value={this.state.inputText}     onChange={this.handleChange} />
    	
    	{checkFalse}

      </div>
    )
  }

}

export default TextInput
```
使用AJAX

1.先在server.js加上app.post的路徑
```
app.post('/hi',function(req,res){
	res.end("hi");
});
```

2.
```

import React, { Component } from 'react'
import Proptest from "./Proptest"



class TextInput extends Component {

  constructor() {
    super()
    this.state = {
      inputText: ' sdst'
    }
    this.handleChange = this.handleChange.bind(this);
    this.deleteLetter = this.deleteLetter.bind(this);
  }






 handleChange(e){
 	console.log(e.target.value);
 	
 	this.setState({inputText:e.target.value});
 }
 deleteLetter(){
 	$.ajax({
	 type:"POST",
      url: "/hi",
      dataType: 'text',
      success: function(data) {
        console.log(data);
      }.bind(this),
      error: function(xhr, status, err) {
        console.error("error");
      }.bind(this)
    });

 	
 	this.setState({
 		inputText:this.state.inputText.substring(0,this.state.inputText.length-1)
 	});
 }






  render() {
  	var checkFalse = true;
  	if(checkFalse){
  		checkFalse =  <Proptest text={this.state.inputText} deleteLetter={this.deleteLetter}/>;
 	}else{
  		checkFalse = <p>This is false</p>
  	}



    return (
      <div>
        
    	<input    value={this.state.inputText}     onChange={this.handleChange} />
    	
    	{checkFalse}

      </div>
    )
  }

}

export default TextInput

```

#使用React router
1.
https://github.com/EasonWang01/react-tutorial

clone後到branch master開始進行

2.
`npm install react-router`

之後開啟client.js

改為下面，看是否仍正常啟動

```
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import { Router, Route, hashHistory } from 'react-router'

render(( 
	<Router history={hashHistory}>
     <Route path="/" component={App}/>
    </Router> 
  ),document.getElementById('app'))


```
再改為下面看看

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
到路徑http://localhost:3000/#/about

即可看到，元件的切換

(發現頁面切換元件很快速，我們以前要做到這樣必須用AJAX，或模板引擎內的動態compile(一樣是AJAX加載)，
但React沒用到ajax，完全都在client端計算更改的virtual DOM後更新到DOM上)

---
接著可到webpack那章，加上commonchunk plugin，加速我們每次網頁重新整理的速度

---
#如何寫style
個人習慣方式為:

1.創一個style資料夾

2.每個component有對應名稱的css檔案

3.之後全部`@import`到index.css內

4.於server設定`app.use(express.static('./style'));`

5.引入到index.html
```
<link rel=stylesheet type="text/css" href="../style.css">
```


#加上Bootstrap
 雖然有react-bootstrap，但我們也可用原本的方式
 
 在client.js加上
 ```
 <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">```
 
 之後在用這個轉換網站轉換後貼上即可，記得外面要包著div
 
 http://facebook.github.io/react/html-jsx.html
  
  


#選取元素
在元素內放入下面的property

`ref={(c) => this._input = c}`

再用`this._input`即可選定該元素

或是

`<input ref="myInput" />`

```
var input = this.refs.myInput;
var inputValue = input.value;
```

Style React

###一般寫法:
把style放在物件裡面
```
  <button style={style.submit} onClick={()=>this.handleSubmit()}>Submit</button>
  
  var style = {
  submit:{background:"green"}
}
```
###使用其他庫

###1.Radium
```
1.import Radium from 'radium'

2.export default Radium(TodoInput) //class名稱

3.var style = {
  submit:{
        ':hover': {
              backgroundColor: 'red'
            }
  }
}
```
http://stack.formidable.com/radium/

###2.Material-ui

`npm install material-ui`

都是個別引入
```
import RaisedButton from 'material-ui/lib/raised-button';

 <RaisedButton label="Submit"onClick={()=>this.handleSubmit()} />
```
http://www.material-ui.com/#/customization/inline-styles

#####PS:如果使用click相關的元件沒反應的話
`npm install react-tap-event-plugin`

```
import injectTapEventPlugin from 'react-tap-event-plugin';

  constructor(props) {
    super(props);
   
    injectTapEventPlugin();
  }
```
參考:https://github.com/callemall/material-ui#react-tap-event-plugin

#Material UI 現在0.15後需如下使用

1.加入Mui 的context

2.injectTapEventPlugin();
```
import React from 'react';
import ReactDOM from 'react-dom';
import { render } from 'react-dom'
import root from './root.js'
import {Provider} from 'react-redux'
import {configureStore} from '../redux/store'
import {Router, browserHistory, Route} from 'react-router';
import MuiThemeProvider from 'material-ui/styles/MuiThemeProvider';
import injectTapEventPlugin from 'react-tap-event-plugin';
const initialState = window.__PRELOADED_STATE__;
injectTapEventPlugin();
const store = configureStore(initialState);

ReactDOM.render(
	<Provider store={store}>
		<MuiThemeProvider>
	    <Router history={browserHistory} routes={root} />
		</MuiThemeProvider>
	</Provider>
,document.getElementById('app')
)

```
##使用server side rendering with Material UI

一樣加入context和injectTapEventPlugin

```
var express = require('express');
var path = require('path');
var config = require('../../webpack.config.js');
var webpack = require('webpack');
var webpackDevMiddleware = require('webpack-dev-middleware');
var webpackHotMiddleware = require('webpack-hot-middleware');

import React from 'react';
import {renderToString} from 'react-dom/server';
import {RouterContext, match, createRoutes} from 'react-router';
import root from '../client/root.js';
import {Provider} from 'react-redux'
import {configureStore} from '../redux/store'
import MuiThemeProvider from 'material-ui/styles/MuiThemeProvider';
import getMuiTheme from 'material-ui/styles/getMuiTheme';
import injectTapEventPlugin from 'react-tap-event-plugin';
injectTapEventPlugin();
const routes = createRoutes(root);


var app = express();

var compiler = webpack(config);

app.use(webpackDevMiddleware(compiler, {noInfo:true,publicPath: config.output.publicPath}));
app.use(webpackHotMiddleware(compiler));
app.use(express.static('./dist'));


app.post('/ajax',function(req,res){

	res.end("success");
})

let initialState = {
		todos:[{
			id:0,
			completed: false,
			text:'initial for demo'
		}]
}


const store = configureStore(initialState);


app.get('*', (req, res) => {
	const muiTheme = getMuiTheme({
	  userAgent: req.headers['user-agent'],
	});
  match({routes, location: req.url}, (error, redirectLocation, renderProps) => {
    if (error) {
      res.status(500).send(error.message);
    } else if (redirectLocation) {
      res.redirect(302, redirectLocation.pathname + redirectLocation.search);
    } else if (renderProps) {
      const content = renderToString(
				<Provider store={store}>
				  <MuiThemeProvider muiTheme={muiTheme}>
					  <RouterContext {...renderProps} />
				  </MuiThemeProvider>
				</Provider>
			);
      let state = store.getState();
      let page = renderFullPage(content, state);
      return res.status(200).send(page);
    } else {
      res.status(404).send('Not Found');
    }
  });
});

const renderFullPage = (html, preloadedState) => (`
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>React Todo List</title>
</head>
<body>
  <div id="app">${html}</div>
  <script>
  window.__PRELOADED_STATE__ = ${JSON.stringify(preloadedState).replace(/</g, '\\x3c')}
   </script>
  <script src="vendor.bundle.js"></script>
  <script src="bundle.js"></script>
</body>
</html>
`
);

var port = 3000;

app.listen(port, function(error) {
  if (error) throw error;
  console.log("Express server listening on port", port);
});

```


#React toggle style

如何在點擊時切換style呢

1

```
 <div className={"flipper" + (this.props.flipped ? " flipped" : "")}>
```
假設如上範例，可以先寫出，一個className的名稱的決定是由其於父組件上的prop決定


之後其父組件使用state來設定其他子組件的prop

```
    render: function() {
        return <div>
            <Flipper flipped={this.state.flipped} orientation="horizontal" />
            <Flipper flipped={this.state.flipped} orientation="vertical" />

            <div className="button-container">
                <button onClick={this.flip}>Flip!</button>
            </div>
        </div>;
```

所以點擊時會觸發下面這個函式

```
  flip: function() {
        this.setState({ flipped: !this.state.flipped });
    },
```

#在render方法內使用js新增component

ex:
```
render (){
<div>
  {addth}
</div>
}
```
在這裡addth可以是兩種寫法

1.一個是直接return出完整的dom element

2.或是把每個dom element放入array中

```
['<th>some<th>','<th>some<th>']
```

第二種方法即是常見的使用map放入的技巧

#findDOMNode

之前版本的getDOMNode已經拿掉

可如下使用findDOMNode

```
findDOMNode(this.refs.chart)
```

#Server side rendering(使用Express)

參考此repo

https://github.com/EasonWang01/React-router-Redux-isomorphic-Boilerplate


#Stateless component

相對於使用class，使用 `const = function` 的方式會提升效能

但其不可使用lifecycle跟state,ref

需要props傳入

使用ref需於parent的class用div寫上ref在於其內引入stateless component

範例:
```
  clickCircle = (e) => {
    this.refs.cir1.children[0].style.background='red';
  }

  <div ref='cir1' key={1} onClick={(e) => this.clickCircle(e)} style={styles.circleContainer}>
    <TwoCircle />
    <div style={styles.p}>粉絲註冊點帳戶</div>
    <div style={styles.number}>6.69</div>
  </div>
```



