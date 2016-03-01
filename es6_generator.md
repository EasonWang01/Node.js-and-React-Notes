# ES6 Generator

#function接上*即是

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

#如沒加上yield
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
#yield不可放在沒有*的function中
```
(function (){
  yield 1;
})()
```
執行會錯誤

#generator函式執行時不會改變，內部變數的值
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

#使用for of
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

#如何在chrome dev 加入strict mode?
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
即可順利遍歷*foo()

#yield*

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

##yield*類似for of
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