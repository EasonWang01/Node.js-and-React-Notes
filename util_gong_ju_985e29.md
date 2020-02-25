# util \(工具類\)

## util \(工具類\)

這是一個輔助的類別，常可幫助簡化程式碼

1.

上一章用到的

## util.inherits\(,\)

但要注意它只會繼承 父類別之後 在原型 _prototype_ 註冊的  _**函數**_

```text
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

目前感覺不是太好用

2.

## util.inspect

檢測一個物件的屬性，可以是function 或object

```text
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

3.

## util.isArray\(object\)

查看是不是陣列

```text
var util = require('util');

console.log(util.isArray([]));

console.log(util.isArray(new Array));

console.log(util.isArray({}));
```

4.

## util.isRegExp\(object\)

查看是不是正規表達式

```text
var util = require('util');

console.log(util.isRegExp(/some regexp/));

console.log(util.isRegExp(new RegExp('another regexp')));

console.log(util.isRegExp({}));
```

5.

## util.isDate\(object\)

查看是不是日期格式

```text
var util = require('util');

console.log(util.isDate(new Date()));

console.log(util.isDate(Date())); //沒有new會返回字串

console.log(util.isDate({}));
```

6.

## util.isError\(object\)

查看是不是錯誤對象

```text
var util = require('util');

util.isError(new Error())
  // true
util.isError(new TypeError())
  // true
util.isError({ name: 'Error', message: 'an error occurred' })
  // false
```

