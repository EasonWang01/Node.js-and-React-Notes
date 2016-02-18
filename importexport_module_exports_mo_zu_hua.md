# import_export_module.exports 模組化
參考至

http://www.2ality.com/2014/09/es6-modules-final.html

http://es6.ruanyifeng.com/#docs/module

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
使用import的格式
```
import {Hello} from './component.jsx';
```

但為什麼react在webpack可以用?

```
import React from 'react';
import ReactDOM from 'react-dom';
```

(因為react是用 export default 進行輸出)
