# JS 基本

## Debounce

在特定時間段內，多次呼叫該 function，僅會在計時結束後執行一次，如果還沒到計時器結束又呼叫，則重新執行計時

> 例如使用者在文字輸入中，等到沒輸入後才改變畫面狀態

```javascript
function debounce(fn, delay = 1000) {
  let timer = null
  return function () {
    timer && clearTimeout(timer)
    timer = setTimeout(() => {
      fn.apply(this, arguments)
      timer = null
    }, delay)
  }
}
const c = debounce(() => {console.log('test')})
// 使用：呼叫多次 c()
```

## Throttle

在特定時間段內，多次呼叫該 function，僅會每次間隔時間執行一次

> 例如使用者在輸入中，但每次固定間隔一段時間才會改變狀態

```javascript
function throttle(fn, delay = 1000) {
  let timer
  return function (...args) {
    if (timer) return
    timer = setTimeout(() => {
      fn.apply(this, args)
      timer = null
    }, delay)
  }
}
const c = throttle(() => {console.log('test')})
// 使用：呼叫多次 c()
```
