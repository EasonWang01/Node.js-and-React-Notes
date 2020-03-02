# assert \(自訂拋出的錯誤\)

## assert \(自訂拋出的錯誤\)

### 一般會用mocha框架替代

如果遇預期相同則不會有錯誤

```text
var assert = require('assert');


assert( 3 === 4, '預期出錯');
```

另一種寫法

```text
assert.equal(4, 3, '预期錯誤');
```

相反

```text
assert.notEqual(4, 3, '预期錯誤');
```

\*\*注意其使用!= 而非!==

如要使用=== 可用

```text
assert.strictEqual()
assert.notStrictEqual()
```

## 比較物件是否相等

```text
var assert = require('assert');

var person1 = { "name":"john", "age":"21" };
var person2 = { "name":"john", "age":"22" };


assert.notDeepEqual(person1, person2, '预期錯誤');
```

反之

```text
assert.deepEqual()
```

## throws\(\) 和 doesNotThrow\(\)，他們用来判斷一段代碼是否會抛出異常

```text
var assert = require('assert');
assert.throws(function() {///預期會拋出
　　throw new Error("Seven Fingers. Ten is too mainstream.");
});
assert.doesNotThrow(function() {///預期不會拋出，但拋出了，所以產生異常訊息
　　throw new Error("I lived in the ocean way before Nemo");
});
```

## assert.fail\(21, 21, 'Testtest Failed', '\#\#\#'\);不管怎樣都會拋出異常

將上面例子改成如下

```text
var assert = require('assert');
assert.throws(function() {///預期會拋出
　　assert.fail(21, 21, 'Testtest Failed', '###');
});
assert.doesNotThrow(function() {///預期不會拋出，但拋出了，所以產生異常訊息
　　assert.fail(21, 21, 'Test Failed', '###');
});
```

