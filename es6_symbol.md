# ES6 Symbol

用途:讓Object中的key名稱不會被覆蓋
```
var a = {}

var g = Symbol('aa')

var v = Symbol('aa')

a[g] = 12
a[v] = 15


Symbol(): 13

Symbol(aa):13453453
Symbol(aa): 12
```