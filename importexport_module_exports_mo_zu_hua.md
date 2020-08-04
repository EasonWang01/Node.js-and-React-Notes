# JS 模組化

## import\_export\_module.exports 模組化

參考至

[http://www.2ality.com/2014/09/es6-modules-final.html](http://www.2ality.com/2014/09/es6-modules-final.html)

[http://es6.ruanyifeng.com/\#docs/module](http://es6.ruanyifeng.com/#docs/module)

在webpack中可混用，一起用

## ES6 module

```text
 //------ lib.js ------
    export sqrt = Math.sqrt;

    //------ main.js ------
    import  sqrt  from 'lib';
```

想一次import所有export過的東西

```text
 import * as lib from 'lib';
```

舉例:

```text
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  };
}

export function removeTodo(id) {
  return {
    type: 'REMOVE_TODO',
    id
  };
}
```

傳回的東西是一個物件

```text
import * as TodoActionCreators from './TodoActionCreators';
console.log(TodoActionCreators);
// {
//   addTodo: Function,
//   removeTodo: Function
// }
```

## CommonJS

```text
exports.foo = function () { ... };

module.exports = config;

var a = require('a');
```

## 使用import的格式

```text
import {Hello} from './component.jsx';
```

但為什麼react在webpack可以用?

```text
import React from 'react';
import ReactDOM from 'react-dom';
```

\(因為react是用 module.exports 進行輸出\)

## 結論

1.

```text
module.exports = class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
}
```

or

```text
export default class extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```

配上

```text
import Hello from './component.jsx';
```

2.

```text
exports.Hello = class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```

or

```text
export class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```

配上

```text
import {Hello} from './component.jsx';
```

## 使用require

```text
module.exports = class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
```

```text
var Helloo = require('./component.jsx');
function main() {
    ReactDOM.render(<Helloo />, document.getElementById('app'));
}
```

## 有關react引入jsx

使用commonjs的寫法require時

```text
function main() {
    ReactDOM.render(<Hllo />, document.getElementById('app'));
}
```

其中是指向

```text
var hllo = require('aaa.jsx')
```

也就是指向命名的var跟你原本元件的名稱沒關係

所以如果你想在一個jsx寫入多個元件會較麻煩，需要使用多個jsx檔案每個寫一個元件，再全部require到一個主jsx內

### 所以推薦使用ES6的import，下面hllo直接指到目標檔案的元件名稱，所以可以在一個jsx檔案內放多個元件

```text
 import {Hllo} from './component.jsx';

main();

function main() {
    ReactDOM.render(<Hllo />, document.getElementById('app'));
}
```

```text
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

原因是ES6的import可以使用{}如果使用會直接引入檔案內的object，如果沒用則是引入export default 或module.exports的檔案

## 或是使用ES6 的exports 記得 require後加上  ".屬性"

```text
import React from 'react';

var style1 = {
  background: 'yellow'
};

exports.a =  class Hello extends React.Component {
  render() {
    return <h1 style={style1}>Hello world,{this.props.name}</h1>;
  }
};
exports.b =  class Hello extends React.Component {
  render() {
    return <h1 style={style1}> world,{this.props.name}</h1>;
  }
};
```

```text
import React from 'react';
import ReactDOM from 'react-dom';
var Hello = require('./component.jsx').b;
main();
console.log(Hello);
function main() {
    ReactDOM.render(<Hello />, document.getElementById('app'));
}
```

