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


