
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

#實際使用
1.快速刪除array重複的元素
```
var arr = [3, 5, 2, 2, 5, 5];
var apple = [...new Set(arr)];

console.log(apple);
```
2.快速合併陣列，刪除重複元素
```
a = [1,2,3,4,5,6]
b = [3,4,5,6,7,8,9]

banana = new Set([...a,...b])
```
3.快速找到array中共同的元素(交集)
```
var a = [1, 2, 3];
var b = new Set([1,3,4,5,8,9]);


banana = new Set([...a].filter(x => b.has(x)));
```
4.快速找到a陣列有但b陣列沒有的元素(差集)
```
var a = [1, 2, 3];
var b = new Set([1,3,4,5,8,9]);


banana = new Set([...a].filter(x => !b.has(x)));
```
#Map

```
var fruit = new Map();
var apple = {price: "20"};

fruit.set(apple, "content")
```
取得值
```
fruit.get(apple);
fruit.get({price: "20"}); //undefined

```
#Map為了避免index衝突，須將index先指定給變數(除了數字，字串和boolean)

```
var map = new Map();

var apple1 = ['a'];
var apple2 = ['a'];

map
.set(apple1, 111)
.set(apple2, 222);

map.get(apple1) 
map.get(apple2) 
```
但
```
map.get(['a']) 
```

為undefined

##Map的操作方法
```
set(key, value)  設值
get(key)         取值
has(key)         查看擁有
delete(key)      清除單一
clear()          清除所有
```
##Map的遍歷方法
```
keys()：返回index
values()：返回value
entries()：返回index and value
forEach()：使用callback附上參數
```
#實際使用
1.
```
 map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);
```
```
[...map.keys()][0]
```
2.
```
var fruit = new Map();
var apple = {price: "20"};

fruit.set(apple, "content")
```
```
[...fruit.keys()][0].price
```