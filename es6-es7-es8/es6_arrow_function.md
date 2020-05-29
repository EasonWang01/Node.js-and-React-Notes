# ES6 Arrow function

## 1.箭頭函式的內部沒有自己的 [arguments](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/arguments)、[super](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/super)、[new.target](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/new.target) 等語法

## 2. 箭頭函式 this 綁定到其寫的時候所在的範圍

而傳統函式的 this 依呼叫的時候存在的範圍而定

## ES6 Arrow function

```javascript
const s = 5
() => s  // 相同於function(){return s};
(() => s)()
```

### return is implicit under some circumstances

#### \(return在箭頭函數內有時可省略\)

```text
// returns: undefined
// explanation: an empty block with an implicit return
((name) => {})() 

// returns: 'Hi Jess'
// explanation: no block means implicit return
((name) => 'Hi ' + name)('Jess')

// returns: undefined
// explanation: explicit return required inside block, but is missing.
((name) => {'Hi ' + name})('Jess')

// returns: 'Hi Jess'
// explanation: explicit return in block exists
((name) => {return 'Hi ' + name})('Jess') 

// returns: undefined
// explanation: a block containing a single label. No explicit return.
// more: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label
((name) => {id: name})('Jess') 

// returns: {id: 'Jess'}
// explanation: implicit return inside expression ( ) returns object
((name) => ({id: name}))('Jess') 

// returns: {id: 'Jess'}
// explanation: explicit return inside block returns object
((name) => {return {id: name}})('Jess')
```

## 注意

如果使用\(\)=&gt;{}加上了{}則箭頭函數不會auto return

```text
var a = () => {12}
undefined
a()
undefined
var a = () => 12
undefined
a()
12
```

來源:[http://stackoverflow.com/questions/28889450/when-should-i-use-return-in-es6-arrow-functions](http://stackoverflow.com/questions/28889450/when-should-i-use-return-in-es6-arrow-functions)

