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

## JS Map Async

因為 map 內有 await 不會按照原本的想法走，所以要用如下

```javascript
const arr = [1, 2, 3];

const asyncRes = await Promise.all(arr.map(async (i) => {
	await sleep(10);
	return i + 1;
}));

console.log(asyncRes);
```

## JS ForEach Async

因為 ForEach 內有 await 不會按照原本的想法走，所以要用如下

```javascript
async function asyncForEach(array, callback) {
  for (let index = 0; index < array.length; index++) {
    await callback(array[index], index, array);
  }
}

asyncForEach(arr, doSomething)
```

