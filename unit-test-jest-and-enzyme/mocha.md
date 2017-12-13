

1.只要throw new Error即可告知測試錯誤

```js
describe('加法測試', function() {
    it('1 加 1 = 2', function() {
      throw new Error('錯誤') 
    });
  });
```

2.只要return即會判斷成功\(不論true 或 false\)

```js
describe('加法測試', function() {
    it('1 加 1 = 2', function() {
      return
    });
  });
```





