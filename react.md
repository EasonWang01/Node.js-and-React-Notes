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
裡面放入package.json
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
 接著在server目錄下新增server.js
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
