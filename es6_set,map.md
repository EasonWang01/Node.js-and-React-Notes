# ES6-Set,Map

for of =>可用在array，Set，Map返回value

for in =>可用在object，array，返回index




##如何遍歷object內的value?
```
var obj = { first: "John", last: "Doe" };
// Visit non-inherited enumerable keys
Object.keys(obj).forEach(function(key) {
    console.log(obj[key]);
});
```



Set的遍歷方法
```

keys()：返回index
values()：返回value
entries()：返回index and value
forEach()：使用callback附上參數
```