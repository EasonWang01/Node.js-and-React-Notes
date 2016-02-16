# assert (自訂拋出的錯誤)

如果遇預期相同則不會有錯誤

```
assert(value, message)
```
範例
```
var assert = require('assert');


assert( 3 === 4, '預期出錯');
```

另一種寫法
```
assert.equal(4, 3, '预期錯誤');
```

