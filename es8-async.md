#### \#Async與Await

可以簡單地在任何function前加上async字樣，之後把裡面會需要異步的function前加上await即可

後面用then\(\)即可去進行步驟控制

```
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

#### 

因為一般function沒有內建promise所以無法用Async，需要如下使用

```js
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



