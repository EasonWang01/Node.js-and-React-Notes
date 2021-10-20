# mocha

## 決定是否測試成功

> 使用it內的function第一個參數done

1.只要throw new Error即可告知測試錯誤

```javascript
describe('加法測試', function() {
    it('1 加 1 = 2', function(done) {
      done(new Error('錯誤'))
    });
  });
```

2\.

> function內只要return即會判斷成功(不論true 或 false)

```javascript
describe('加法測試', function() {
    it('1 加 1 = 2', function(done) {
       done()
    });
  });
```

## 異步測試(Async)

> 1.可用this.timeout(1000)設定要容忍的回應時間
>
> 1. 如果寫在describe層級則所有it皆會納用
> 2. 如果寫在it層級額只有該層it會使用
> 3. 如果寫Arrow function則無法用this要先綁定外層

```javascript
describe('加法測試', function() {
    //this.timeout(4000);
    it('1 加 1 = 2', function (done){
        this.timeout(6000);
      setTimeout(() => {

        done()
      },3000)
    });
    it('1 加 1 = 2', function (done){
          this.timeout(6000);
        setTimeout(() => {

          done()
        },5000)
      });
  });
```

##

## 測試報表UI

[http://adamgruber.github.io/mochawesome/](http://adamgruber.github.io/mochawesome/)

> 產生測試後的結果並輸出到網頁上

```
npm install --save-dev mochawesome
mocha testfile.js --reporter mochawesome
```

然後會產生一個資料夾裡面包含html檔案和json檔案
