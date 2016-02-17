# import_export_module.exports 模組化
參考至

http://www.2ality.com/2014/09/es6-modules-final.html

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



