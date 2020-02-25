# ES6 Generator

# function接上\*即是

開啟REPL

```
function* Fruit() {
  yield 'apple';
  yield 'banana';
  return 'ending';
}

var a = Fruit();
```

執行

```
a.next()
```

另外試試

```
Fruit().next()
```

發現如直接對函式下next指令會無法往下遍歷

# 如沒加上yield

```
function* f() {
  console.log('執行！');
  console.log('執行！');
  console.log('執行！');
}

var a = f()
```

執行

```
a.next()
```

發現函式一次執行完畢

# yield不可放在沒有\*的function中

```
(function (){
  yield 1;
})()
```

執行會錯誤

# generator函式執行時不會改變內部變數的值\(yield應該放在每行的前面\)

```
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(10);
```

執行

```
a.next();
```

發現到第二次z即不知道y的值

所以要改成

```
b.next()
a.next(22/3) 
a.next(10+(22/3)+22)
```

雖然要手動指定參數，但我們一般在函式內需要使用依賴變數不會用generator

如何讀取上次yield的變數?

```
function* foo(x) {
  yield y = 2 * ( x + 1);
  yield z =  (y / 3);
  return (x + y + z);
}

var a = foo(10);
```

```
a.next();//執行三次
```

發現順利計算

但y跟z變全域，因為yield後接var會出錯，而javascript預設也是在function執行完後釋放區域變數，所以本來的設定是yield後如果要變數保存，就必須設他為全域

```
yield var y = 2 * ( x + 1);
```

# 使用for of

```
function *foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}
```

```
for (let v of foo()) {
  console.log(v);
}
```

執行for of後發現chrome告知只有在strict才可執行

# 如何在chrome dev 加入strict mode?

```
(function() { "use strict"; 

//放入你要執行的東西
}());
```

所以我們改成

```
(function() { "use strict"; 

for (let v of foo()) {
  console.log(v);
 }

}());
```

即可順利遍歷\*foo\(\)

# yield\*

在generator函式中插入另一個generator函式時  
用到

```
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  foo();
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
```

印出x,y

```
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
```

印出x,a,b,y

## yield\*類似for of

```
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  for(let v of foo()){
  console.log(v);
  };
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
```

處理callback

原本

```
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // Do something with value4
      });
    });
  });

});
```

使用generator

```
function* longRunningTask() {
  try {
    var value1 = yield step1();
    var value2 = yield step2(value1);
    var value3 = yield step3(value2);
    var value4 = yield step4(value3);
    // Do something with value4
  } catch (e) {
    // Handle any error from step1 through step4
  }
}
```

為任意對象加入Iterator後即可用next\(\)

```
var arry = ["a","b"]
```

```
var hi = arry[Symbol.iterator](); //S記得大寫
```

```
hi.next();
```

## 測試一個物件對象

```
var car = {type:"sport", model:"500", color:"white"};
```

接著輸入

```
for(v of car){console.log(v)}
```

發現物件對象無法用for of遍歷

於是我們再輸入，幫他加入iterator

```
car[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
```

這時輸入

```
for(v of car){console.log(v)}
```

以及

```
[...car]
```

均可

但輸入next還是不行

```
car.next()
```

所以我們生成一個b

```
var b = car[Symbol.iterator]()
```

之輸入b.next\(\)，即可順利印出

```
b.next()
Object {value: 1, done: false}
b.next()
Object {value: 2, done: false}
b.next()
Object {value: 3, done: false}
```

但如果改成

```
var b = car[Symbol.iterator]
```

之後輸入

```
b().next()
Object {value: 1, done: false}
b().next()
Object {value: 1, done: false}
b().next()
Object {value: 1, done: false}
```

發現第一個完後即無法在往下

# 查看一個數組有沒有iterator

```
Set.prototype[Symbol.iterator]

Array.prototype[Symbol.iterator]

Map.prototype[Symbol.iterator]

Object.prototype[Symbol.iterator]
```

# 有\[Symbol.iterator\]的方法都可用\[...名字\]去遍歷

只有object無法

# yield 實用方法

在Promise中我們會在async function call的完成狀態\(onSuccess\)調用`resolve(要傳下去的值)`，而在Generator中我們會調用`g.next(要傳下去的值);`其中next裡面放的是接下來要繼續處理的值

```
var g = gen(); // 建立函數物件
g.next(); // 開始執行到第一個 yield

var totalDelay = 0;

function delay(ms) {
  setTimeout(function() {
    console.log("delay "+ms+" ms");
    totalDelay += ms;
    g.next(totalDelay); // 這個 yield 完成後，傳回 totalDelay 並呼叫 next() 繼續執行到下一個 yield
  }, ms);
}

function *gen(){
  var a = 5, b = 3, t;
  t = yield delay(800);
  console.log("a=%d t=%d", a, t);
  t = yield delay(500);
  b = a + b;
  console.log("b = %d t = %d", b, t);
  t = yield delay(300);
  console.log("totalDelay = %d t=%d", totalDelay, t);
}
```

可參考[http://programmermagazine.github.io/mag/pmag201505/focus3.html](http://programmermagazine.github.io/mag/pmag201505/focus3.html)

