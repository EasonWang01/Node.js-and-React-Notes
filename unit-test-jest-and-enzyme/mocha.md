1.只要throw new Error即可告知測試錯誤

```js
describe('加法測試', function() {
    it('1 加 1 = 2', function(done) {
      done(new Error('錯誤'))
    });
  });
```

2.只要return即會判斷成功\(不論true 或 false\)

```js
describe('加法測試', function() {
    it('1 加 1 = 2', function(done) {
       done()
    });
  });
```

# 測試報表UI

[http://adamgruber.github.io/mochawesome/](http://adamgruber.github.io/mochawesome/)

> 產生測試後的結果並輸出到網頁上

```
npm install --save-dev mochawesome
mocha testfile.js --reporter mochawesome
```

然後會產生一個資料夾裡面包含html檔案和json檔案

