# TCP

網路七層協議  
[https://zh.wikipedia.org/wiki/OSI模型](https://zh.wikipedia.org/wiki/OSI模型)

TCP的特色在於傳輸資料時，會有握手的過程，以確保雙方身份，所以花的時間多一點。

而UDP的特色在於傳輸資料時，不需要驗  
證資料，不保證正確性，發送端不知道數據是否會正確接收，所以速度較快速

一般瀏覽網頁時使用的協議是HTTP與HTTPS，其主要是基於TCP，為TCP往上之發展

# Node.js中的TCP

在node.js主要使用`net`這個核心模組來提供TCP的相關功能，

一般主要是在做與硬體溝通時會使用到

具有TCP中的TCP server與 TCP client的兩種類型

#### 實作

1.進入資料夾第10章中的TCP資料夾，執行test1.js來執行TCP server

2.開啟另一個terminal，一樣進入資料夾第10章中的TCP資料夾，執行test2.js

3.結合Repl

將client test2.js改為如下

```
var net = require('net');

var HOST = 'localhost';
var PORT = 8000;

var client = new net.Socket(); //建立一個新的socket實例
client.connect(PORT, HOST, function() {

    console.log('CONNECTED TO: ' + HOST + ':' + PORT);
    client.write('hello,this is from client!');//發送給server數據


    const repl = require('repl');
    var test = repl.start('請輸入: ').context;
    test.hello = function() {
      client.write('client說了hello!');
    }
    //之後啟動client後輸入hello()
});

client.on('data', function(data) {
    console.log('DATA: ' + data);
});

client.on('close', function() {
    console.log('Connection closed');
});
```

# 



