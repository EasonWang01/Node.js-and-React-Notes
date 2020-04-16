# ES8 Async

可以簡單地在任何function前加上async字樣，之後把裡面會需要異步的function前加上await即可

後面用then\(\)即可去進行步驟控制

```javascript
async function getTrace () {  
    pageContent = await fetch('www.google.com', {
      method: 'get'
    })
  return pageContent
}

getTrace()  
  .then((data) => {
    console.log('----')
    console.log(data)
  })
```

因為一般function沒有內建promise所以無法用Async，需要如下使用

```javascript
function timeout(ms) {
    return new Promise(resolve => setTimeout(() => {
    resolve();
    console.log(111)
  }, ms));
}
async function sleep() {
    await timeout(3000);
    console.log(123)
}
```

## Async 後的 function 也會變promise 化

```javascript
function timeout(ms) {
    return new Promise(resolve => setTimeout(() => {
    resolve();
  }, ms));
}
async function sleep() {
    await timeout(3000);
    console.log(123)
}

await sleep()
console.log('end')
```

## Async function內的return等同於resolve

`reject`則為 `throw error`

```javascript
function timeout(ms) {
    return new Promise(resolve => setTimeout(() => {
    resolve();
  }, ms));
}
async function sleep() {
    await timeout(3000);
    console.log(123)
    return 'sleep'
}

const c = await sleep()
console.log(c)
```

