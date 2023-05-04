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

## Call, Apply, Bind

`call`, `apply` 和 `bind` 都是用來改變函式的 `this` 值的方法。

1.  `call()` 方法：它允許您指定函式中的 `this` 值以及一個參數列表。這些參數會作為個別的引數傳遞給函式。`call()` 方法的語法如下：

    ```vbnet
    function.call(thisArg, arg1, arg2, ...)
    ```
2.  `apply()` 方法：它也允許您指定函式中的 `this` 值，但它的參數必須是一個數組（或類似數組的對象）。這個數組中的元素會作為引數傳遞給函式。`apply()` 方法的語法如下：

    ```arduino
    function.apply(thisArg, [argsArray])
    ```
3.  `bind()` 方法：它會返回一個新的函式，其中 `this` 值被綁定到指定的對象。`bind()` 方法的語法如下：

    ```javascript
    function.bind(thisArg, arg1, arg2, ...)
    ```

    這個方法會創建一個新函式，其中 `this` 值被綁定到 `thisArg`，而 `arg1`, `arg2`, ... 這些參數則被作為該函式的引數。綁定後的函式可以稍後調用，並且 `this` 值將保持綁定的對象。

`call()` 和 `apply()` 的作用是改變函式的 `this` 值並立即調用函式，而 `bind()` 的作用是創建一個新的函式，並將 `this` 值綁定到指定的對象上。

## 深拷貝、淺拷貝

淺拷貝（Shallow Copy）是指在複製一個對象時，新對象只會複製原始對象中的基本數據類型和一級屬性的引用。也就是說，新對象和原始對象中的一級屬性（如字符串、數字、布林等）會共享同一個引用。如果修改了其中一個對象中的屬性，另一個對象中的屬性也會隨之改變。

```
Object.assign()
展開運算符（spread operator）obj2 = { ...obj1 };
Array.slice()
```

深拷貝（Deep Copy）是指在複製一個對象時，新對象會複製原始對象中的所有數據，包括所有層級的嵌套對象和數組。也就是說，新對象和原始對象中的所有屬性都是獨立的，不會相互影響。

```
使用 JSON.parse() 和 JSON.stringify() 方法
使用遞歸實現深拷貝
```

\
