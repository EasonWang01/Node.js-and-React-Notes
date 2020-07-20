# ES6 Symbol

用途:讓Object中的key名稱不會被之後取相同名字的key覆蓋

```text
var a = {}

var g = Symbol('aa')

var v = Symbol('aa')

a[g] = 12
a[v] = 15


Symbol(): 13

Symbol(aa):13453453
Symbol(aa): 12
```

之後必須用a\[g\]來取值，不可用a.g

## 實用處

```javascript
const actions = {
  fetch: "FETCH",
  get: "GET" 
}

// 以前的寫法可改為

const actions = {
  fetch: Symbol(),
  get: Symbol()
}


switch ...
case: action.fetch
....
```

