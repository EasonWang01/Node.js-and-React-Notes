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

#有關react引入jsx
使用commonjs的寫法require時
```
function main() {
    ReactDOM.render(<Hllo />, document.getElementById('app'));
}
```
其中<Hllo />是指向
```
var hllo = require('aaa.jsx')
```
 也就是指向命名的var跟你原本元件的名稱沒關係
 
 所以如果你想在一個jsx寫入多個元件會較麻煩，需要使用多個jsx檔案每個寫一個元件，再全部require到一個主jsx內
 
 ##所以推薦使用ES6的import，下面hllo直接指到目標檔案的元件名稱，所以可以在一個jsx檔案內放多個元件
 
 ```
 import {Hllo} from './component.jsx';

main();

function main() {
    ReactDOM.render(<Hllo />, document.getElementById('app'));
}
 ```
 ```
export class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
};

export class Hllo extends React.Component {
  render() {
    return <h1 style={style1}>Hello,{this.props.name}</h1>;
  }
}
export class Hllo extends React.Component {
  render() {
    return <h1 style={style1}>Hello,{this.props.name}</h1>;
  }
}
 ```