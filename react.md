# React
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

ReactDOM.render(<HelloMessage name="Sebastian" />, mountNode);
```
更多有關ES5 react to ES6 or ES7
http://cheng.logdown.com/posts/2015/09/29/converting-es5-react-to-es6

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
另一種寫法，是直接在DOM 的onchange中綁，但官方推薦綁在constructor
```
onChange={this.handleChange.bind(this)}
```
####!每次改動constructor記得都要重新整理，就算有用Hot reload

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

新增一個元件為Propest.js
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