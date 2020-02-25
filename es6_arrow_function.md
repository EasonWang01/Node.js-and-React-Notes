# ES6 Arrow function

```
var s = 5
```
```
()=> s  //相同於function(){return s};
```

```
(()=> s)()
```
##return is implicit under some circumstances
###(return在箭頭函數內有時可省略)

```
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
#注意

如果使用()=>{}加上了{}則箭頭函數不會auto return
```
var a = () => {12}
undefined
a()
undefined
var a = () => 12
undefined
a()
12

```


來源:http://stackoverflow.com/questions/28889450/when-should-i-use-return-in-es6-arrow-functions