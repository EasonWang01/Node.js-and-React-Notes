# import_export_module.exports 模組化
參考至

http://www.2ality.com/2014/09/es6-modules-final.html

http://es6.ruanyifeng.com/#docs/module

在webpack中可混用，一起用

# ES6 module



```
 //------ lib.js ------
    export sqrt = Math.sqrt;

    //------ main.js ------
    import  sqrt  from 'lib';
 
```
想一次import所有export過的東西
```
 import * as lib from 'lib';
```
#CommonJS
```
exports.foo = function () { ... };

module.exports = config;

var a = require('a');

```
#使用import的格式
```
import {Hello} from './component.jsx';
```

但為什麼react在webpack可以用?

```
import React from 'react';
import ReactDOM from 'react-dom';
```

(因為react是用 module.exports 進行輸出)

#結論

1.

```
module.exports = class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
}
```
or
```
export default class extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```
配上
```
import Hello from './component.jsx';
```

2.
```
exports.Hello = class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```
or
```
export class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```
配上
```
import {Hello} from './component.jsx';

```

#使用require

```
module.exports = class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```
```
var Helloo = require('./component.jsx');
function main() {
    ReactDOM.render(<Helloo />, document.getElementById('app'));
}

```
