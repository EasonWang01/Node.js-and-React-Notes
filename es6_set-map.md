# ES6 Set,Map

## 前情提要

for of =&gt;可用在array，Set，Map返回value

for in =&gt;可用在object，array，返回index\(在array可能被擴充的情況下不建議使用\)

forEach =&gt;均可遍歷，但無法break離開遍歷\(可用some或是every\) [http://www.jsnoob.com/2013/11/26/how-to-break-the-foreach/](http://www.jsnoob.com/2013/11/26/how-to-break-the-foreach/)

### 如何遍歷object內的value?

```text
var obj = { first: "John", last: "Doe" };
// Visit non-inherited enumerable keys
Object.keys(obj).forEach(function(key) {
    console.log(obj[key]);
});
```

第二種

```text
Object.keys(this.state.users).map(function(objectKey, index) {
let value = objectKey;
return (
  <div>{value}</div>
)
})
```

## ES6 的新數據結構Set Map

### Set

可接受一個array來初始化，set類似array但每個成員的值不會相等，使用強型別

```text
var set = new Set([1, 2, 3, 4, 4])
```

輸入set 將得到

```text
Set {1, 2, 3, 4}
```

### Set 操作方法

```text
add(value)：添加value，返回Set本身。
delete(value)：删除value，返回boolean。
has(value)：返回boolean，表示該value是否為set中成員。
clear()：清除所有成員，没有返回值。
```

1.

```text
var a = new Set

a.add(1);
```

2.

```text
a.add("1")
```

### Set的遍歷方法

```text
keys()：返回index
values()：返回value
entries()：返回index and value
forEach()：使用callback附上參數
```

#### 想用array的操作方法來操作Set呢?

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype)

可將

將Set轉為arrray

```text
var items = new Set([1, 2, 3, 4, 5]);
var array = Array.from(items);
```

## 實際使用

1.快速刪除array重複的元素

```text
var arr = [3, 5, 2, 2, 5, 5];
var apple = [...new Set(arr)];

console.log(apple);
```

2.快速合併陣列，刪除重複元素

```text
a = [1,2,3,4,5,6]
b = [3,4,5,6,7,8,9]

banana = new Set([...a,...b])
```

3.快速找到array中共同的元素\(交集\)

```text
var a = [1, 2, 3];
var b = new Set([1,3,4,5,8,9]);


banana = new Set([...a].filter(x => b.has(x)));
```

4.快速找到a陣列有但b陣列沒有的元素\(差集\)

```text
var a = [1, 2, 3];
var b = new Set([1,3,4,5,8,9]);


banana = new Set([...a].filter(x => !b.has(x)));
```

## Map

```text
var fruit = new Map();
var apple = {price: "20"};

fruit.set(apple, "content")
```

取得值

```text
fruit.get(apple);
fruit.get({price: "20"}); //undefined
```

## Map為了避免index衝突，須將index先指定給變數\(除了數字，字串和boolean\)

```text
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

```text
map.get(['a'])
```

為undefined

### Map的操作方法

```text
set(key, value)  設值
get(key)         取值
has(key)         查看擁有
delete(key)      清除單一
clear()          清除所有
```

### Map的遍歷方法

```text
keys()：返回index
values()：返回value
entries()：返回index and value
forEach()：使用callback附上參數
```

## 實際使用

```text
使用[...]轉為陣列結構
```

1.

```text
 map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);
```

```text
[...map.keys()][0]
```

2.

```text
var fruit = new Map();
var apple = {price: "20"};

fruit.set(apple, "content")
```

```text
[...fruit.keys()][0].price
```

```text
陣列轉為Map
```

1.

```text
new Map([[true, 7], [{foo: 3}, ['abc']]])
```

```text
陣列轉為JSON
```

1.

```text
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
```

