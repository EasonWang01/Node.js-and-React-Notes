# ES6 Promise

1.Promise為構造函數，使用new生成
```
promise = new Promise(function(){})
```

2.Promise可以傳入一個為函數的參數，該函數擁有兩個參數

3.這兩個參數均為函數，分別為resolve()和reject()
```
var promise = new Promise(function(resolve, reject) {
  //first execute 

  if (/* 如first execute成功 */){
    resolve(value);//發出resolve
  } else {
    reject(error);發出reject
  }
});
```

4.使用resolve()代表成功，使用reject()代表promise function沒成功

5.promise生成後可用then，執行成功或失敗鎖要執行的東西
```
promise.then(function(value) {
  // success//接收到resolve後會執行
}, function(value) {
  // failure //接收到reject後會執行
});
```

----
#自己創造Promise函式

使用new Promise，裡面參數放入想要async處理的function。

將new Promise放在一個之後會調用then  的function名稱的return中

```
function get(url) {

  return new Promise(function(resolve, reject) {
   
    var req = new XMLHttpRequest();
    req.open('GET', url);

    req.onload = function() {
     
      if (req.status == 200) {
       
        resolve(req.response);
      }
      else {
      
        reject(Error(req.statusText));
      }
    };

    req.onerror = function() {
      reject(Error("Network Error"));
    };

    req.send();
  });
}
```
調用方法
```
get('story.json').then(function(response) {
  return JSON.parse(response);
}).then(function(response) {
  console.log("Yey JSON!", response);
});
```