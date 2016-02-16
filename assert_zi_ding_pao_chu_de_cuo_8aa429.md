# assert (自訂拋出的錯誤)

如果遇預期相同則不會有錯誤


```
var assert = require('assert');


assert( 3 === 4, '預期出錯');
```

另一種寫法
```
assert.equal(4, 3, '预期錯誤');
```
相反

```
assert.notEqual(4, 3, '预期錯誤');
```
**注意其使用!= 而非!==

如要使用=== 可用
```
assert.strictEqual()
assert.notStrictEqual()
```

#比較物件是否相等

```
var assert = require('assert');

var person1 = { "name":"john", "age":"21" };
var person2 = { "name":"john", "age":"22" };


assert.notDeepEqual(person1, person2, '预期錯誤');
```

反之
```
assert.deepEqual()
```