# util (工具類)

這是一個輔助的類別，常可幫助簡化程式碼

## util.promisify()

將 function 轉為 promise

```javascript
 util.promisify((arg, resolve, reject) => {
   doSomething(foo, resolve);
 });
 // 原本假設 doSomething 要執行第二個參數當作 
 //  callback 時就會去執行 resolve
```

或是可以單純這樣寫

```javascript
 util.promisify(fs.stat);
```

## util.inherits()

但要注意它只會繼承 父類別之後 在原型 _prototype_ 註冊的 _ **函數**_

```
var util = require('util'); 
function Base() { 

    this.sayHello = function() { 
    console.log('Hello ' + this.name); 
    }; 
} 
Base.prototype.showName = "as";

function Sub() { 
    this.name = 'sub'; 
} 

util.inherits(Sub,Base); 


var dd = new Sub();
console.log(dd.showName);
console.log(Sub.prototype.sayHello);
```

## util.inspect

檢測一個物件的屬性，可以是function 或object

```
var util = require('util'); 
function Person() { 
    this.name = 'byvoid'; 
    this.toString = function() { 
    return this.name; 
    }; 
} 
var obj = new Person(); 

var as = {
a:12

};
console.log(util.inspect(as));
console.log(util.inspect(Person,true));  //如果沒有true只會顯示他是個function
```



## util.isArray(object)

查看是不是陣列

```
var util = require('util');

console.log(util.isArray([]));

console.log(util.isArray(new Array));

console.log(util.isArray({}));
```



## util.isRegExp(object)

查看是不是正規表達式

```
var util = require('util');

console.log(util.isRegExp(/some regexp/));

console.log(util.isRegExp(new RegExp('another regexp')));

console.log(util.isRegExp({}));
```



## util.isDate(object)

查看是不是日期格式

```
var util = require('util');

console.log(util.isDate(new Date()));

console.log(util.isDate(Date())); //沒有new會返回字串

console.log(util.isDate({}));
```



## util.isError(object)

查看是不是錯誤對象

```
var util = require('util');

util.isError(new Error())
  // true
util.isError(new TypeError())
  // true
util.isError({ name: 'Error', message: 'an error occurred' })
  // false
```
