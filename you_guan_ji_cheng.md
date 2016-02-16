# 有關繼承

似乎將
```
Dog.prototype.__proto__ = EventEmitter.prototype;
```
改成
```
Dog.prototype= EventEmitter.prototype;

Dog.prototype =new EventEmitter();
```
都可以跑，但這樣不是應該會蓋掉Dog的prototype，而找不到name屬性報錯嗎

```
var EventEmitter = require('events').EventEmitter;

function Dog(name) {
  this.name = name;
}

Dog.prototype.__proto__ = EventEmitter.prototype;
// 另一种写法
// Dog.prototype = Object.create(EventEmitter.prototype);

var simon = new Dog('simon');

simon.on('bark', function(){
  console.log(this.name + ' barked');
});

setInterval(function(){
  simon.emit('bark');
}, 500);
```

#以上的解釋是
EventEmitter的屬性是後來用
```
EventEmitter.prototype.xxx="bbb"
```
去定義的，而不是一開始的建構子
----------------

而下面這三個，只讀的到後來用EventEmitter.prototype.xxx定義的東西
```
Dog.prototype.__proto__ = EventEmitter.prototype;

Dog.prototype= EventEmitter.prototype;

 Dog.prototype = Object.create(EventEmitter.prototype);

```
想要繼承到建構子跟後來定義的prototype只能用以下這個方法
```
Dog.prototype =new EventEmitter();
```


#可看下面的範例
```

var EventEmitter = require('events').EventEmitter;

function Dog(name) {
	
  this.name = "name";
};
function DDog(name) {
	
  this.aabb = "ass";
};
DDog.prototype.aa="aaaaa";
Dog.prototype=new DDog();

var aaa = new Dog();
console.log(aaa.aa);


```
#而使用Dog.prototype= DDog.prototype;也不會覆蓋原本的Dog建構子
```

function Dog(name) {
	
  this.name = "name";
};
function DDog(name) {
	
  this.aabb = "ass";
};
DDog.prototype.aa="aaaaa";
Dog.prototype= DDog.prototype;

var aaa = new Dog();
console.log(aaa.name);

```
#避免將函式內的屬性設為name
```

function Daaog(as) {
	
};
console.log(Daaog.name);///沒有name但一樣會有console
```
其他預設屬性也不要使用，拿來命名屬性
```
Function.arguments
Function.arity
Function.caller
Function.displayName
Function.length
Function.name
Function.prototype
```
#使用prototype指定屬性後要用new出新物件後才可使用

--------------------
#delete可用來刪除函式中的方法
```
delete User.save;
```
如果要刪除prototype加上prototype即可，其子代的該方法也被一併刪除
```
delete User.prototype.save; 

```
#ES5    With Object.create
```
var a = {a: 1}; 
// a ---> Object.prototype ---> null

var b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 (inherited)

var c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

var d = Object.create(null);
// d ---> null
console.log(d.hasOwnProperty); 
// undefined, because d doesn't inherit from Object.prototype
```

更多可參考

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain