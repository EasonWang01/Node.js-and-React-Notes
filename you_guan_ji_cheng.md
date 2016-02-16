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
想要讀到建構子跟後來定義的prototype只能用以下這個方法
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