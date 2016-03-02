
#前情提要
-----

for of =>可用在array，Set，Map返回value

for in =>可用在object，array，返回index(在array可能被擴充的情況下不建議使用)

forEach =>均可遍歷，但無法break離開遍歷(可用some或是every)
http://www.jsnoob.com/2013/11/26/how-to-break-the-foreach/


##如何遍歷object內的value?
```
var obj = { first: "John", last: "Doe" };
// Visit non-inherited enumerable keys
Object.keys(obj).forEach(function(key) {
    console.log(obj[key]);
});
```

#ES6 的新數據結構Set Map
-----
##Set
可接受一個array來初始化，set類似array但每個成員的值不會相等，使用強型別
````
var set = new Set([1, 2, 3, 4, 4])
```
輸入set 將得到
```
Set {1, 2, 3, 4}
```
##Set 操作方法
```
add(value)：添加value，返回Set本身。
delete(value)：删除value，返回boolean。
has(value)：返回boolean，表示該value是否為set中成員。
clear()：清除所有成員，没有返回值。
```
1.
```
var a = new Set

a.add(1);
```
2.
```
a.add("1")
```

##Set的遍歷方法
```

keys()：返回index
values()：返回value
entries()：返回index and value
forEach()：使用callback附上參數
```
###想用array的操作方法來操作Set呢?
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype

可將

將Set轉為arrray
```
var items = new Set([1, 2, 3, 4, 5]);
var array = Array.from(items);
```